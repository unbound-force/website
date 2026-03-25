---
title: "Knowledge Retrieval with Dewey"
description: "Set up Dewey for semantic knowledge retrieval — install, configure content sources, and give your AI agents cross-repo context."
lead: "Give your AI agents the context they need. Install Dewey, configure your knowledge sources, and enable semantic search across your entire organization."
date: 2026-03-25T00:00:00+00:00
draft: false
weight: 80
toc: true
---

## What is Dewey?

Dewey is a semantic knowledge layer that gives AI agents rich, cross-repository context. It combines a structured knowledge graph (page traversal, tag queries, wikilink navigation) with vector-based semantic search — so agents can find conceptually related content even when different terminology is used. A search for "authentication timeout" finds an issue titled "login session expiry" because Dewey understands meaning, not just keywords.

Dewey runs as an MCP (Model Context Protocol) server alongside your AI coding environment. It indexes your local repository, pulls issues and PRs from GitHub, crawls toolstack documentation from the web, and makes all of it searchable through a unified interface. The result: every hero in the swarm — from Muti-Mind writing specs to Cobalt-Crush implementing features — makes better decisions because they have better context.

Dewey is a hard fork of [graphthulhu](https://github.com/skridlevsky/graphthulhu), preserving all existing knowledge graph capabilities and adding persistent storage, semantic search, and pluggable content sources. If you were using graphthulhu, Dewey is a drop-in replacement — existing agent configurations work without prompt changes.

## Installation

Install Dewey and its embedding model:

```bash
# Install Dewey
brew install --cask unbound-force/tap/dewey

# Install Ollama (local model runtime) and pull the embedding model
brew install --cask ollama
ollama pull granite-embedding:30m
```

The `granite-embedding:30m` model is IBM's Granite Embedding — a 63 MB model licensed under Apache 2.0 with full training data transparency. It runs locally via Ollama; no data leaves your machine.

If the Homebrew formula is not yet available, install from source:

```bash
go install github.com/unbound-force/dewey@latest
```

## Getting Started

Initialize Dewey in your repository:

```bash
# Create .dewey/ directory with default configuration
dewey init

# Edit the source configuration
# (see Source Configuration below for examples)
$EDITOR .dewey/sources.yaml

# Build the initial index (local files + any configured sources)
dewey index

# Verify everything is working
dewey status
```

The `dewey status` command reports index health: page count, block count, embedding coverage, and source status. A healthy output shows all configured sources indexed with recent timestamps.

After initialization, Dewey persists its indexes to `.dewey/graph.db` (SQLite). Subsequent sessions load from the persistent index and only re-process changed files — startup is near-instant after the first index.

## Source Configuration

Dewey indexes content from three pluggable source types. Configure them in `.dewey/sources.yaml`:

### Local Disk

The foundational source — indexes all Markdown files in your repository, including hidden directories (`.opencode/`, `.specify/`, `.muti-mind/`). This is enabled by default after `dewey init`.

Local files are watched for changes in real time. When you save a file, Dewey re-indexes only the changed content.

### GitHub API

Fetches issues, pull requests, READMEs, and documentation from whitelisted repositories in your GitHub organization:

```yaml
sources:
  - id: github-unbound-force
    type: github
    name: unbound-force
    config:
      org: unbound-force
      repos:
        - gaze
        - website
        - homebrew-tap
    refresh_interval: daily
```

Authentication uses your existing `gh` CLI credentials — the same mechanism that `mutimind sync` uses for GitHub issue synchronization.

### Web Crawl

Fetches and indexes documentation from toolstack websites so agents can reference current API docs for your dependencies:

```yaml
sources:
  - id: web-go-stdlib
    type: web
    name: go-stdlib
    config:
      urls:
        - https://pkg.go.dev/std
      depth: 2
    refresh_interval: weekly

  - id: web-cobra-docs
    type: web
    name: cobra-docs
    config:
      urls:
        - https://cobra.dev/
      depth: 1
    refresh_interval: weekly
```

Web crawls respect `robots.txt`, impose a configurable delay between requests, and cache content locally in `.dewey/cache/`. Content is only re-fetched when the refresh interval expires.

### Updating Sources

External sources (GitHub, web) are updated explicitly — not on every session start:

```bash
# Update all sources
dewey index

# Update a specific source by ID
dewey index --source github-unbound-force
dewey index --source web-go-stdlib
```

This separates the "fetch external content" operation (which may take seconds to minutes) from the "start serving queries" operation (which is near-instant from the persistent index).

## OpenCode Integration

Dewey integrates with OpenCode as an MCP server. Add this to your `opencode.json`:

```json
{
  "mcp": {
    "dewey": {
      "type": "local",
      "command": ["dewey", "serve", "--vault", "."],
      "enabled": true
    }
  }
}
```

If you use `uf init` to scaffold your project, this configuration is generated automatically — you do not need to add it manually.

Once configured, all hero agents can use Dewey's MCP tools for knowledge retrieval. The tools include structured queries (`search`, `find_by_tag`, `query_properties`, `get_page`, `traverse`, `find_connections`) and semantic queries (`dewey_semantic_search`, `dewey_similar`, `dewey_semantic_search_filtered`).

## Graceful Degradation

Dewey is an enhancement, not a requirement. Per the Unbound Force constitution's Composability First principle, every hero functions without Dewey — they just have less context to work with.

| Tier           | What's Available                                          | What You Get                                                                                                         |
| -------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Full Dewey** | Semantic search + structured graph + external sources     | Rich cross-repo context, conceptual similarity search, toolstack documentation, GitHub issue discovery               |
| **Graph-only** | Structured graph queries (no Ollama / no embedding model) | Keyword search, tag queries, wikilink traversal, property queries — equivalent to graphthulhu                        |
| **No Dewey**   | Direct file reads and CLI queries                         | Heroes read local files directly. Functional but with narrower context — no cross-repo awareness, no semantic search |

All heroes check for Dewey's availability at runtime. If Dewey is not configured or not running, they fall back gracefully — reduced context quality, but fully functional. You can adopt Dewey incrementally: start with local disk indexing, add GitHub sources when you want cross-repo context, add web crawl when you want toolstack docs.

## Next Steps

- Read the [Dewey tool page](/docs/team/dewey/) for architecture details, query capabilities, and how each hero uses Dewey
- See [Common Workflows](/docs/getting-started/common-workflows/) for how Dewey provides context during the hero lifecycle
- Explore the role-specific guides to see how [developers](/docs/getting-started/developer/), [testers](/docs/getting-started/tester/), [product owners](/docs/getting-started/product-owner/), and [product managers](/docs/getting-started/product-manager/) each use Dewey
