---
title: "Your AI Agent Doesn't Read the Docs"
description: "AI coding agents generate code from stale training data. Here's how semantic search over current API docs fixes that."
lead: "Your AI agent writes code using training data snapshots. Your project's dependencies ship updates weekly. That gap costs you time."
slug: "dewey-knowledge-retrieval"
date: 2026-04-01T00:00:00+00:00
draft: false
weight: 70
toc: true
categories: ["Engineering"]
tags: ["dewey", "knowledge-retrieval", "semantic-search", "documentation"]
contributors: ["Unbound Force"]
---

## The Problem with Training Data

When you use a framework, you keep the docs open in a browser tab. You check function signatures, read about new APIs, and verify that the patterns you remember are still current. You do this because libraries evolve — the "right way" to do something in React, Go, or TypeScript changes with each release.

Your AI coding agent does not do this. It generates code from a training data snapshot that may be weeks or months old. It does not check whether the API it is calling still exists, whether the function signature has changed, or whether a better pattern was introduced in a recent release.

This creates a specific, observable failure mode: the agent produces code that _looks correct_ but uses deprecated APIs, outdated patterns, or function signatures that have changed. The code compiles. It might even pass basic tests. But it carries a maintenance burden because it is built on stale assumptions.

Some examples of what this looks like in practice:

- **Go**: An agent uses `sort.Slice` when `slices.SortFunc` (introduced in Go 1.21) is the current idiomatic approach. It uses `io/ioutil.ReadFile` when `os.ReadFile` replaced it in Go 1.16.
- **React**: An agent generates class components with `componentDidMount` when function components with `useEffect` have been the standard for years. It uses `ReactDOM.render` when `createRoot` replaced it in React 18.
- **TypeScript**: An agent writes manual type guards when the `satisfies` operator (introduced in TypeScript 4.9) handles the pattern more cleanly. It uses `enum` when `as const` objects are the recommended pattern.

These are not hallucinations — the agent is accurately reproducing patterns it learned. The patterns are outdated.

## What a Human Developer Does Differently

When you encounter an unfamiliar API or want to verify a pattern, you open the documentation. You search for the function name, read the current signature, check the examples, and write code that matches what the docs say today — not what you remember from six months ago.

The fix for AI agents is the same: give them access to the documentation.

## How Dewey Bridges the Gap

[Dewey](/docs/getting-started/knowledge/) is a semantic knowledge layer that indexes content from three source types and makes it available through semantic search:

- **Disk sources** index Markdown files in your local repositories — specs, READMEs, design documents, and agent configurations
- **GitHub sources** pull issues, PRs, and READMEs from your organization's repositories — giving agents context about decisions, discussions, and known bugs
- **Web sources** crawl documentation from toolstack websites — current API references, framework guides, and library documentation

When an agent asks "how do I sort a slice in Go," the answer comes from the current `pkg.go.dev/std` documentation, not from training data. When it needs to set up a Cobra CLI command, it references the current `cobra.dev` documentation, not a pattern it memorized during training.

The key insight is that **web sources are the most impactful customization** you can make to Dewey. Disk and GitHub sources are auto-configured by [`uf init`](/docs/getting-started/knowledge/#what-uf-init-creates), but web sources are project-specific — only you know which frameworks and libraries your project depends on.

## Before and After

Consider an agent working on a Go project that uses Cobra for CLI commands and GORM for database access.

**Without web sources**, the agent works from training data. It generates a Cobra command using the pattern it learned during training — which may use an older initialization style, miss new flags, or structure the command tree differently than the current Cobra documentation recommends. When it writes a GORM query, it might use deprecated `Find` patterns instead of the current `Session` API.

**With web sources** for `cobra.dev` and `gorm.io/docs`, the agent's semantic search finds the relevant documentation pages when it encounters these patterns. The query "how to define a Cobra subcommand" returns the current documentation, not a training-era snapshot. The generated code matches the current API surface.

The difference is not always dramatic — sometimes the training data is current enough. But for projects with active dependencies that release frequently, the gap between "training-era patterns" and "current-docs patterns" compounds over time. Fixing stale API usage in code review is slower than preventing it in the first place.

## Adding Web Sources

Adding web sources to Dewey takes about two minutes. Open `.dewey/sources.yaml` and add entries for your project's key dependencies:

```yaml
# Go standard library reference
- id: web-go-stdlib
  type: web
  name: go-stdlib
  config:
    urls:
      - https://pkg.go.dev/std
    depth: 2
  refresh_interval: weekly

# CLI framework
- id: web-cobra
  type: web
  name: cobra-cli-framework
  config:
    urls:
      - https://cobra.dev/
    depth: 1
  refresh_interval: weekly
```

Then run `dewey index` to fetch and index the new content. Subsequent sessions load from the persistent index — the agent's context is richer without any startup delay.

The [knowledge retrieval guide](/docs/getting-started/knowledge/#extending-your-sources) has complete templates for Go and TypeScript projects, plus guidance on choosing stable documentation URLs and setting crawl depth.

## Getting Started

Install the Unbound Force toolchain:

```bash
brew install unbound-force/tap/unbound-force
uf setup
```

`uf setup` installs Dewey and runs `uf init`, which auto-detects sibling repos and your GitHub org to generate a multi-repo source config. To add web sources for your toolstack, edit `.dewey/sources.yaml` and run `dewey index`.

See the [Quick Start guide](/docs/getting-started/quick-start/) for detailed installation, or the [knowledge retrieval guide](/docs/getting-started/knowledge/#extending-your-sources) for the full source configuration reference.
