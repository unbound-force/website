---
title: "Dewey"
description: "Semantic knowledge retrieval for AI agents — structured graph traversal plus vector-based semantic search across local files, GitHub, and web documentation."
lead: "Semantic knowledge retrieval for AI agents."
date: 2026-03-26T00:00:00+00:00
draft: false
weight: 30
toc: true
---

## What Dewey Does

AI agents make better decisions when they have better context. Dewey is an MCP server that gives agents unified access to your organization's knowledge — local Markdown files, GitHub issues and PRs, and web documentation — through both structured graph queries and vector-based semantic search.

A search for "authentication timeout" finds an issue titled "login session expiry" because Dewey understands meaning, not just keywords. Agents start with a vague concept and refine their understanding through structured navigation, discovering related specifications, past decisions, and connected documentation they did not know to ask for.

Dewey is a hard fork of [graphthulhu](https://github.com/skridlevsky/graphthulhu), building on its knowledge graph foundation and adding persistent storage, semantic search, and pluggable content sources. Dewey now provides 48 MCP tools across 12 categories.

## Installation

```bash
# macOS (Homebrew)
brew install --cask unbound-force/tap/dewey

# Install Ollama and pull the embedding model
brew install --cask ollama
ollama pull granite-embedding:30m
```

On Linux, install from source:

```bash
go install github.com/unbound-force/dewey@latest
```

## Key Features

### 48 MCP Tools Across 12 Categories

Dewey exposes 48 MCP tools for navigate, search, analyze, write, decision, journal, flashcard, whiteboard, semantic search, health, knowledge management, and learning. Agents use these tools through the standard MCP protocol — no custom integrations required.

### Semantic Search

Vector-based similarity search using IBM Granite embeddings (30M parameters, 63 MB, Apache 2.0). Finds conceptually related content even when different terminology is used. Runs locally via Ollama — no data leaves your machine.

### Four Content Source Types

- **Local disk** — indexes all Markdown files in your repository with real-time file watching
- **GitHub API** — fetches issues, PRs, READMEs, and documentation from whitelisted repositories
- **Web crawl** — indexes documentation from toolstack websites with robots.txt compliance and local caching
- **Code** — Go AST parsing to extract function signatures, CLI commands, MCP tool registrations, and package documentation from Go source files

### Persistent SQLite Index

Indexes persist to `.uf/dewey/graph.db` across sessions. Subsequent startups load from the persistent index and only re-process changed files — startup is near-instant after the first index.

### Graceful Degradation

Dewey is an enhancement, not a requirement. Every hero in the swarm functions without Dewey, falling back to direct file reads. Semantic search requires Ollama; structured graph queries work without it.

### Knowledge Lifecycle

Dewey does not just store knowledge — it actively curates it. Three commands manage the lifecycle of stored learnings:

- **Compile** (`dewey compile`) — clusters stored learnings by topic and synthesizes them into current-state articles using an LLM. Raw learnings are an append-only event log; compiled articles are materialized views rebuilt from those learnings whenever new information arrives.
- **Lint** (`dewey lint`) — scans the knowledge base for quality issues: stale decisions, uncompiled learnings, embedding gaps, and contradictions between sources. Some issues are mechanically fixable; others require compilation or human judgment.
- **Promote** (`dewey promote`) — moves content between trust tiers (draft to validated to authored). Promotion requires human review, ensuring that machine-generated knowledge is vetted before it influences critical decisions.

This lifecycle means Dewey's knowledge base improves over time. Each session adds raw learnings; compilation distills them into coherent articles; linting catches quality issues before they propagate; and promotion ensures reviewed content ranks higher in search results.

## Workflow Impact

Dewey's semantic context enables a fundamental shift in how the Unbound Force swarm operates. With Dewey configured, the [Product Owner (Muti-Mind)](/docs/getting-started/product-owner/) can draft specifications autonomously -- retrieving related context from across the organization, writing the spec with acceptance criteria, self-clarifying using Dewey instead of asking the human, and validating against historical patterns.

This reduces human checkpoints from two to one. Instead of writing specifications and accepting results, the human provides a short seed (1-2 sentences of intent) and reviews the completed increment. Everything between seed and accept -- specification, planning, implementation, testing, and review -- is handled by the swarm.

See [Common Workflows](/docs/getting-started/common-workflows/#new-feature-end-to-end) for the full hero lifecycle with autonomous define.

## Architecture

Dewey runs as an MCP server alongside your AI coding environment. It combines:

- **Knowledge graph** — in-memory graph built from Markdown files with wikilink, tag, and property relationships
- **SQLite persistence** — pages, blocks, links, and embeddings stored in `.uf/dewey/graph.db`
- **Ollama embeddings** — IBM Granite model generates vector embeddings for semantic similarity search
- **Pluggable sources** — content source interface supports disk, GitHub, web crawl, and code with configurable refresh intervals

## Learn More

- [Getting Started Guide](/docs/getting-started/knowledge/) — install, configure sources, and integrate with OpenCode
- [Dewey Team Page](/docs/team/dewey/) — query capabilities, embedding model details, and how each hero uses Dewey
- [GitHub Repository](https://github.com/unbound-force/dewey) — source code, issues, and releases
