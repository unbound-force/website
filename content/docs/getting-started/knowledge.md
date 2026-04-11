---
title: "Knowledge Retrieval with Dewey"
description: "Set up Dewey for semantic knowledge retrieval — install, configure content sources, and give your AI agents cross-repo context."
lead: "Give your AI agents the context they need. Install Dewey, configure your knowledge sources, and enable semantic search across your entire organization."
date: 2026-03-25T00:00:00+00:00
draft: false
weight: 80
toc: true
---

## What Dewey Does

Dewey is a semantic knowledge layer that gives AI agents rich, cross-repository context. It combines a structured knowledge graph (page traversal, tag queries, wikilink navigation) with vector-based semantic search — so agents can find conceptually related content even when different terminology is used. A search for "authentication timeout" finds an issue titled "login session expiry" because Dewey understands meaning, not just keywords.

Dewey runs as an MCP (Model Context Protocol) server alongside your AI coding environment. It indexes your local repository, pulls issues and PRs from GitHub, crawls toolstack documentation from the web, and makes all of it searchable through a unified interface. The result: every hero in the swarm makes better decisions because they have better context. Most significantly, Dewey enables [Muti-Mind](/docs/getting-started/product-owner/) to draft specifications autonomously -- reducing human checkpoints from two to one by giving the Product Owner agent sufficient context to define features from a short seed of intent.

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

### Embedding Model Alignment

The Unbound Force swarm and Dewey are aligned on the same embedding model (IBM Granite `granite-embedding:30m`). To ensure consistency for processes spawned outside of `uf setup` (e.g., `dewey serve`, manual `replicator init`), add these environment variables to your shell profile (`~/.zshrc` or `~/.bashrc`):

```bash
export OLLAMA_MODEL=granite-embedding:30m
export OLLAMA_EMBED_DIM=256
```

`uf setup` sets these automatically during installation. The shell profile entries ensure they persist across terminal sessions. Without them, child processes may use different embedding models, causing inconsistent search results between the swarm and Dewey.

If the Homebrew formula is not yet available, install from source:

```bash
go install github.com/unbound-force/dewey@latest
```

## Initialize Your Repository

Initialize Dewey in your repository:

```bash
# Create .uf/dewey/ directory with default configuration
dewey init

# Edit the source configuration
# (see Source Configuration below for examples)
$EDITOR .uf/dewey/sources.yaml

# Build the initial index (local files + any configured sources)
dewey index

# Verify everything is working
dewey status
```

The `dewey status` command reports index health: page count, block count, embedding coverage, and source status. A healthy output shows all configured sources indexed with recent timestamps.

After initialization, Dewey persists its indexes to `.uf/dewey/graph.db` (SQLite). Subsequent sessions load from the persistent index and only re-process changed files — startup is near-instant after the first index.

## What `uf init` Creates

If you used `uf setup` or `uf init` to scaffold your project, Dewey's source configuration was generated automatically. Understanding what was created helps you extend it effectively.

`uf init` runs three steps for Dewey initialization:

1. `dewey init` — creates the `.uf/dewey/` directory with a bare default config (single disk source pointing to `.`)
2. `generateDeweySources()` — overwrites `sources.yaml` with an auto-detected multi-repo config
3. `dewey index` — indexes all configured sources

The auto-detection scans your workspace and generates sources based on what it finds:

- **Sibling repo detection**: Scans `../` for directories containing `.git/` — each becomes a separate disk source with fine-grained provenance
- **Org-level workspace**: Adds `../` as a disk source to capture org-level files (design papers, shared documentation)
- **GitHub org extraction**: Reads your `git remote get-url origin` to determine the GitHub org, then creates a GitHub API source for issues, PRs, and READMEs across the detected repos

Here is an example of what `uf init` generates for a project with two sibling repos:

```yaml
# Auto-generated by uf init. Customize as needed.
# This file is user-owned — uf init will not
# overwrite it after initial creation.

sources:
  # Per-repo disk sources (fine-grained provenance)
  - id: disk-local
    type: disk
    name: my-project
    config:
      path: "."

  - id: disk-shared-lib
    type: disk
    name: shared-lib
    config:
      path: "../shared-lib"

  - id: disk-api-service
    type: disk
    name: api-service
    config:
      path: "../api-service"

  # Org-level files (design papers, plans)
  - id: disk-org
    type: disk
    name: org-workspace
    config:
      path: "../"

  # GitHub API (issues, PRs, READMEs)
  - id: github-org
    type: github
    name: my-org
    config:
      org: my-org
      repos:
        - my-project
        - shared-lib
        - api-service
    refresh_interval: daily
```

If you have no sibling repos, the config contains only the current repo's disk source and the GitHub API source (if a GitHub remote exists). If no GitHub remote is detected, the GitHub source is omitted. The auto-detection degrades gracefully — you always get a valid, working config.

### Ownership After Creation

`sources.yaml` is **user-owned** after initial creation. When `uf init` runs again (e.g., after a version update), it checks whether you have customized the file by counting `- id:` entries. If more than one entry exists (meaning the auto-detection already ran or you edited it), `uf init` skips regeneration entirely. Your customizations are never overwritten.

If you ran `dewey init` directly (without `uf init`), you have the bare default — a single disk source pointing to `.`. You can either run `uf init` to get the auto-detected config, or manually add sources using the reference below.

## Configure Content Sources

Dewey indexes content from four pluggable source types. Configure them in `.uf/dewey/sources.yaml`:

### Local Disk

The foundational source — indexes Markdown files in your repository. This is enabled by default after `dewey init`.

Dewey automatically respects `.gitignore` at the source root — no configuration needed. Directories like `node_modules/`, `vendor/`, and other `.gitignore`-excluded paths are skipped during indexing. Hidden directories used by tools (`.git/`, `.obsidian/`, `.uf/dewey/`) are always skipped regardless of `.gitignore`. Dewey-specific directories (`.opencode/`, `.specify/`, `.uf/muti-mind/`) are indexed normally unless excluded by `.gitignore`.

You can add additional ignore patterns and control recursion depth via `sources.yaml`:

```yaml
sources:
  - id: disk-local
    type: disk
    name: my-project
    config:
      path: "."
      ignore:
        - "*.generated.go"
        - "testdata/"
      recursive: true
```

| Field       | Type       | Default | Description                                                 |
| ----------- | ---------- | ------- | ----------------------------------------------------------- |
| `ignore`    | `[]string` | `[]`    | Additional ignore patterns (union-merged with `.gitignore`) |
| `recursive` | `bool`     | `true`  | When `false`, restricts indexing to top-level files only    |

The `ignore` patterns use the same syntax as `.gitignore` — directory names, globs, negation, and comments are all supported.

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

Web crawls respect `robots.txt`, impose a configurable delay between requests, and cache content locally in `.uf/dewey/cache/`. Content is only re-fetched when the refresh interval expires.

Complex documentation sites such as pkg.go.dev are fully supported by the web crawler.

### Code

Indexes Go source files using AST parsing to extract function signatures, CLI commands, MCP tool registrations, and package documentation. This gives agents access to the actual API surface of Go projects without relying on external documentation.

```yaml
sources:
  - id: code-my-project
    type: code
    name: my-project-api
    config:
      path: "../my-go-project"
```

| Field  | Type     | Default | Description                          |
| ------ | -------- | ------- | ------------------------------------ |
| `path` | `string` | —       | Path to the Go module root directory |

Code sources are re-indexed when `dewey index` is run. Only Go files are parsed; non-Go files in the directory are ignored.

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

## Extending Your Sources

`uf init` gives you disk and GitHub sources automatically, but **web sources must be added manually** because they are project-specific. This is the single most impactful customization you can make — it gives your AI agents access to current API documentation for the frameworks and libraries your project depends on.

Without web sources, agents generate code from training data that may be months old. A Go agent might use `sort.Slice` when `slices.SortFunc` is the current idiom. A React agent might generate class components when function components with hooks are the standard. Adding web sources for your toolstack means agents reference the current docs, not a training snapshot.

### Go Project Template

For a Go project, add sources for the standard library and your key frameworks:

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

# ORM / database
- id: web-gorm
  type: web
  name: gorm-orm
  config:
    urls:
      - https://gorm.io/docs/
    depth: 2
  refresh_interval: weekly
```

### TypeScript / JavaScript Project Template

For a TypeScript or JavaScript project:

```yaml
# TypeScript handbook
- id: web-typescript
  type: web
  name: typescript-handbook
  config:
    urls:
      - https://www.typescriptlang.org/docs/handbook/
    depth: 2
  refresh_interval: weekly

# React documentation
- id: web-react
  type: web
  name: react-docs
  config:
    urls:
      - https://react.dev/reference/
    depth: 2
  refresh_interval: weekly

# Next.js documentation
- id: web-nextjs
  type: web
  name: nextjs-docs
  config:
    urls:
      - https://nextjs.org/docs/
    depth: 2
  refresh_interval: weekly
```

### Choosing URLs and Settings

- **Use stable documentation roots**, not specific page URLs. `https://pkg.go.dev/std` is stable; `https://go.dev/doc/effective_go` may move.
- **Set `depth` based on doc structure**: `depth: 1` for single-page docs sites, `depth: 2` for multi-level documentation trees. Higher values crawl more pages but take longer to index.
- **Use `refresh_interval: weekly`** as a default — most documentation sites don't change daily.
- **Check `dewey doctor`** if you see fetch errors — the URL may have changed or the site may block crawlers.

After adding web sources, run `dewey index` to fetch and index the new content. Subsequent indexes only re-fetch when the refresh interval expires.

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

## Diagnostic and Maintenance Commands

### `dewey doctor`

Run `dewey doctor` to check the health of your Dewey installation. It reports on 7 diagnostic sections:

| Section                 | What It Checks                                             |
| ----------------------- | ---------------------------------------------------------- |
| **Environment**         | Vault path, dewey binary location                          |
| **Workspace**           | `.uf/dewey/` directory, config files, `sources.yaml`       |
| **Database**            | `graph.db` health, page/block/embedding counts             |
| **Sources in Database** | Per-source page counts                                     |
| **Embedding Layer**     | Ollama availability, model status                          |
| **MCP Server**          | Lock file, `opencode.json` configuration                   |
| **Summary**             | Overall health with emoji markers (✓ pass, ⚠ warn, ✗ fail) |

```bash
dewey doctor
```

### `dewey reindex`

Deletes `graph.db` and re-indexes from scratch. Use when the index is corrupted or stale:

```bash
dewey reindex
```

This is a destructive operation — the existing index is deleted before rebuilding. For incremental updates, use `dewey index` instead.

### Global CLI Flags

These flags are available on all Dewey commands:

| Flag              | Short | Description                                                               |
| ----------------- | ----- | ------------------------------------------------------------------------- |
| `--verbose`       | `-v`  | Enable debug logging                                                      |
| `--log-file PATH` |       | Write logs to file (auto-logging to `.uf/dewey/dewey.log` also available) |
| `--no-embeddings` |       | Skip embedding generation (useful for quick indexing without Ollama)      |
| `--vault PATH`    |       | Specify vault directory (default: current directory)                      |

## Graceful Degradation

Dewey is an enhancement, not a requirement. Per the Unbound Force constitution's Composability First principle, every hero functions without Dewey — they just have less context to work with.

| Tier           | What's Available                                          | What You Get                                                                                                         |
| -------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Full Dewey** | Semantic search + structured graph + external sources     | Rich cross-repo context, conceptual similarity search, toolstack documentation, GitHub issue discovery               |
| **Graph-only** | Structured graph queries (no Ollama / no embedding model) | Keyword search, tag queries, wikilink traversal, property queries — equivalent to graphthulhu                        |
| **No Dewey**   | Direct file reads and CLI queries                         | Heroes read local files directly. Functional but with narrower context — no cross-repo awareness, no semantic search |

All heroes check for Dewey's availability at runtime. If Dewey is not configured or not running, they fall back gracefully — reduced context quality, but fully functional. You can adopt Dewey incrementally: start with local disk indexing, add GitHub sources when you want cross-repo context, add web crawl when you want toolstack docs.

## Knowledge Lifecycle

Dewey provides three commands that manage the quality and evolution of stored knowledge over time.

### `dewey compile`

Clusters stored learnings by topic and synthesizes them into current-state articles using an LLM. Raw learnings are treated as an append-only event log — they are never deleted. Compiled articles are materialized views, rebuilt from the underlying learnings whenever new information arrives.

```bash
# Compile all learnings into articles
dewey compile

# Compile learnings for a specific tag
dewey compile --tag authentication
```

### `dewey lint`

Scans the knowledge base for quality issues: stale decisions, uncompiled learnings, embedding gaps, and contradictions between sources. Mechanical issues (like missing embeddings) can be fixed automatically with the `--fix` flag; semantic issues require compilation or human judgment.

```bash
# Check for quality issues
dewey lint

# Auto-fix mechanical issues
dewey lint --fix
```

### `dewey promote`

Moves content between trust tiers — from draft to validated, or from validated to authored. Promotion requires human review to ensure machine-generated knowledge is vetted before it influences critical decisions.

```bash
# Promote a draft article to validated
dewey promote --id gotcha-003
```

## Trust Tiers

Dewey classifies all stored knowledge into three trust tiers. Tiers affect how content ranks in search results and allow agents to filter by quality level.

| Tier          | Meaning                             | How Content Gets This Tier                                                    |
| ------------- | ----------------------------------- | ----------------------------------------------------------------------------- |
| **Authored**  | Human-written content               | Content created directly by humans (specs, READMEs, design docs)              |
| **Validated** | Machine-generated, human-reviewed   | Content generated by agents and approved by a human via `dewey promote`       |
| **Draft**     | Machine-generated, not yet reviewed | Content stored by agents via `store_learning` or generated by `dewey compile` |

Higher-tier content ranks above lower-tier content in search results. The `dewey_semantic_search_filtered` tool accepts a `tier` parameter, allowing agents to restrict searches to validated or authored content when reliability matters.

## Storing and Retrieving Learnings

Agents store learnings in Dewey using the `store_learning` MCP tool. Each learning is a natural language paragraph tagged for retrieval and classification.

### Parameters

| Parameter     | Type     | Required | Description                                                                |
| ------------- | -------- | -------- | -------------------------------------------------------------------------- |
| `information` | `string` | Yes      | The learning text — a natural language paragraph describing the insight    |
| `tag`         | `string` | Yes      | A topic tag for grouping and retrieval (e.g., `authentication`, `gotcha`)  |
| `category`    | `string` | No       | Classification: `decision`, `pattern`, `gotcha`, `context`, or `reference` |

### Response

The tool returns a `{tag}-{sequence}` identity string (e.g., `gotcha-003`), which uniquely identifies the stored learning for future reference.

### Search Result Metadata

When retrieving learnings via `dewey_semantic_search` or `dewey_semantic_search_filtered`, results include additional metadata fields:

| Field        | Description                                              |
| ------------ | -------------------------------------------------------- |
| `created_at` | Timestamp when the learning was stored                   |
| `category`   | The learning's classification (if provided at storage)   |
| `tier`       | The trust tier of the content (authored/validated/draft) |

Use the `tier` parameter on `dewey_semantic_search_filtered` to filter results by trust level — for example, restricting to `authored` or `validated` content when making critical decisions.

## Troubleshooting

Common issues and how to resolve them:

| Issue                  | Symptoms                               | Resolution                                                                                                                                  |
| ---------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| MCP server timeout     | OpenCode shows connection timeout      | Check `.gitignore` for large directories being indexed; run `dewey reindex`                                                                 |
| Ollama not running     | `dewey doctor` shows embedding layer ✗ | Run `ollama serve` or install Ollama (`brew install --cask ollama`)                                                                         |
| Lock file conflicts    | "Another dewey instance is running"    | Only one `dewey serve` per vault; check for stale `.uf/dewey/.dewey.lock`                                                                   |
| Low embedding coverage | Semantic search returns few results    | Run `dewey index` to generate embeddings for new content                                                                                    |
| Slow startup           | First `dewey serve` takes minutes      | Normal for large repos on first index; subsequent startups are near-instant. Check `.gitignore` to exclude `node_modules/`, `vendor/`, etc. |

If `dewey doctor` shows failures, start by addressing the ✗ items — each diagnostic section includes enough context to identify the root cause.

## Next Steps

- Read the [Dewey tool page](/docs/team/dewey/) for architecture details, query capabilities, and how each hero uses Dewey
- See [Common Workflows](/docs/getting-started/common-workflows/) for how Dewey provides context during the hero lifecycle
- Explore the role-specific guides to see how [developers](/docs/getting-started/developer/), [testers](/docs/getting-started/tester/), [product owners](/docs/getting-started/product-owner/), and [product managers](/docs/getting-started/product-manager/) each use Dewey
