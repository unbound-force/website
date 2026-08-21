---
title: "Knowledge Retrieval with Dewey"
slug: "knowledge"
description: "Set up Dewey for semantic knowledge retrieval — install, configure content sources, and give your AI agents cross-repo context."
lead: "Give your AI agents the context they need. Install Dewey, configure your knowledge sources, and enable semantic search across your entire organization."
date: 2026-03-25T00:00:00+00:00
draft: false
weight: 80
toc: true
---

## Why Semantic Memory?

AI agents perform best when given concrete context — real file paths, real decisions, real patterns from prior work. Unbound Force delivers context through three tiers: static documentation (AGENTS.md), versioned rules ([convention packs](/docs/getting-started/developer/#convention-packs)), and dynamic semantic memory. Dewey is the third tier — a searchable knowledge layer that provides agents with cross-repo context, prior learnings, and architectural decisions before they start work. In harness engineering terms, Dewey is a feedforward control: it shapes agent behavior by providing context before action, rather than correcting output after.

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

On Linux (Fedora, RHEL, CentOS), download the `.rpm` package from the [Releases page](https://github.com/unbound-force/dewey/releases) and install with `dnf`:

```bash
sudo dnf install ./dewey_<version>_linux_amd64.rpm
```

RPM packages are available for both `amd64` and `arm64` architectures. The binary installs to `/usr/bin/dewey`.

The `granite-embedding:30m` model is IBM's Granite Embedding — a 63 MB model licensed under Apache 2.0 with full training data transparency. It runs locally via Ollama; no data leaves your machine.

> **Note**: macOS Homebrew cask install issues that affected v3.1.0 and v3.2.0 (SHA-256 mismatch errors) have been fixed. If you previously encountered install failures, retry with the latest version.

Pulling the embedding model is recommended but not strictly required to start using Dewey. If the model is not available, Dewey continues in keyword-only mode — structured graph queries, tag lookups, and keyword search all work. Semantic search becomes available once the model is pulled. See [Graceful Degradation](#graceful-degradation) for details.

### RPM (Fedora, RHEL, CentOS)

RPM packages are published with every Dewey release on GitHub, available for both `amd64` and `arm64` architectures. Download the package for your platform from the [latest release](https://github.com/unbound-force/dewey/releases/latest) and install:

```bash
sudo dnf install ./dewey_<version>_linux_amd64.rpm
```

The RPM installs the `dewey` binary to `/usr/bin/dewey`.

### Install from Source

If pre-built packages are not available for your platform, install from source:

```bash
go install github.com/unbound-force/dewey/v3@latest
```

### Embedding Model Alignment

The Unbound Force swarm and Dewey are aligned on the same embedding model (IBM Granite `granite-embedding:30m`). To ensure consistency for processes spawned outside of `uf setup` (e.g., `dewey serve`, manual `replicator init`), add these environment variables to your shell profile (`~/.zshrc` or `~/.bashrc`):

```bash
export OLLAMA_MODEL=granite-embedding:30m
export OLLAMA_EMBED_DIM=256
export DEWEY_CHUNK_MAX_CHARS=12288
```

| Variable | Default | Description |
| -------- | ------- | ----------- |
| `OLLAMA_MODEL` | `granite-embedding:30m` | Embedding model name passed to Ollama |
| `OLLAMA_EMBED_DIM` | `256` | Embedding vector dimension |
| `DEWEY_CHUNK_MAX_CHARS` | `12288` | Maximum chunk size (in characters) for embedding. Overrides the `embedding.max_chunk_chars` config value when set. |
| `DEWEY_EMBEDDING_ENDPOINT` | — | Overrides the Ollama endpoint for embedding requests. Takes highest precedence (see [Endpoint Resolution](#endpoint-resolution) below). |
| `DEWEY_SYNTHESIS_ENDPOINT` | — | Overrides the Ollama endpoint for synthesis (compilation, curation) requests. Fallback chain: `DEWEY_SYNTHESIS_ENDPOINT` → `OLLAMA_HOST` → `http://localhost:11434`. |
| `DEWEY_AUTHOR` | — | Author tag for learning identities (e.g., `alice`). Used in CI or shared environments to attribute learnings to a specific author. |
| `OLLAMA_HOST` | — | Standard Ollama environment variable. Dewey reads this as a fallback when `DEWEY_EMBEDDING_ENDPOINT` is not set and no `embedding.endpoint` is configured in `config.yaml`. |

> **Synthesis vs. embedding precedence**: Synthesis endpoint resolution uses an inverted precedence compared to embedding. For synthesis, `config.yaml` settings take highest priority over environment variables (`config.yaml` > `DEWEY_SYNTHESIS_ENDPOINT` > `OLLAMA_HOST` > default). For embedding, environment variables take highest priority (`DEWEY_EMBEDDING_ENDPOINT` > `config.yaml` > `OLLAMA_HOST` > default). This means setting `DEWEY_SYNTHESIS_ENDPOINT` has no effect if `synthesis.endpoint` is set in `config.yaml`.

`uf setup` sets `OLLAMA_MODEL` and `OLLAMA_EMBED_DIM` automatically during installation. The shell profile entries ensure they persist across terminal sessions. Without them, child processes may use different embedding models, causing inconsistent search results between the swarm and Dewey.

### Endpoint Resolution

Dewey resolves the Ollama endpoint using a 4-tier precedence chain. The first value found wins:

1. **`DEWEY_EMBEDDING_ENDPOINT`** environment variable — highest precedence, intended for CI or custom deployment overrides
2. **`embedding.endpoint`** in `config.yaml` — per-vault configuration, then global configuration
3. **`OLLAMA_HOST`** environment variable — standard Ollama variable, shared with other tools in the Ollama ecosystem
4. **`http://localhost:11434`** — default fallback when nothing else is configured

Values without a scheme (e.g., `192.168.1.50:11434`) are automatically normalized with `http://`.

Most users do not need to set any of these — Dewey connects to `localhost:11434` by default, which is where Ollama listens. Set `OLLAMA_HOST` if you run Ollama on a remote machine or non-standard port, and `DEWEY_EMBEDDING_ENDPOINT` only if Dewey needs a different endpoint than other Ollama clients.

### Provider Configuration

Dewey supports pluggable providers for both embedding and synthesis operations. Configure providers in your vault's `config.yaml` (`.uf/dewey/config.yaml`):

**Ollama (default — local, privacy-preserving)**:

```yaml
embedding:
  provider: ollama
  model: granite-embedding:30m
  endpoint: http://localhost:11434

synthesis:
  provider: ollama
  model: granite3.2:2b
  endpoint: http://localhost:11434
```

**Vertex AI (cloud — Google Cloud Platform)**:

```yaml
embedding:
  provider: vertex
  project: my-gcp-project
  region: us-east5
  model: text-embedding-005

synthesis:
  provider: vertex
  project: my-gcp-project
  region: us-east5
  model: gemini-2.0-flash
```

Vertex AI requires `gcloud` authentication. Run `gcloud auth application-default login` before using Vertex AI providers.

The `region` field accepts either a specific GCP region (e.g., `us-east5`) or `global`. When set to `global`, Dewey uses the `aiplatform.googleapis.com` endpoint without a region prefix, routing requests to the nearest available region.

### Global Configuration

Dewey supports a global configuration file at `~/.config/dewey/config.yaml`. Per-vault configs (`.uf/dewey/config.yaml`) override global settings, allowing you to set organization-wide defaults while customizing individual projects.

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
3. `dewey index --no-embeddings` — indexes all configured sources without generating embeddings, for faster initialization. To generate embeddings for semantic search after init, run `dewey index` separately (see [`--no-embeddings`](#global-cli-flags) in the CLI flags table)

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

The index pipeline is optimized for large vaults: embeddings are generated in batches (batch size 32) rather than per-block, and content sources are fetched concurrently. These optimizations mean indexing time scales sub-linearly with vault size.

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

## Content Sanitization

When indexing content from untrusted sources, Dewey applies a 4-layer sanitization pipeline to protect your knowledge base:

| Layer | What It Checks | Severity |
| ----- | -------------- | -------- |
| **Injection Pattern Scanning** | 10 regex patterns detecting prompt injection, system prompt overrides, and role manipulation | Critical / High |
| **Content Hash Drift** | Detects unexpected changes in previously indexed content | Medium |
| **Markdown Structure Validation** | Validates Markdown structure to catch malformed or adversarial content | Medium |
| **Size Anomaly Detection** | Flags documents that deviate by more than 3 standard deviations from the source's mean size | Medium |

Configure sanitization per-source in `sources.yaml`:

```yaml
sources:
  - id: web-external-docs
    type: web
    name: external-docs
    sanitize_mode: strict   # warn | strict | off
    trust_tier: untrusted   # authored | validated | draft | untrusted
    config:
      urls:
        - https://external-docs.example.com/
```

- **`sanitize_mode: strict`** — blocks content that triggers critical or high severity patterns
- **`sanitize_mode: warn`** — logs findings but indexes the content (default)
- **`sanitize_mode: off`** — skips sanitization entirely

> **Security warning**: Setting `sanitize_mode: off` disables all content scanning. Only use this for fully trusted sources where you control the content. Incorrect `trust_tier` assignment (e.g., marking untrusted content as `authored`) can cause unvalidated content to rank higher in search results.

Sanitization findings are surfaced by `dewey doctor` and `dewey lint`, making it easy to audit your content pipeline.

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

Run `dewey doctor` to check the health of your Dewey installation. It reports on 8 diagnostic sections:

| Section                 | What It Checks                                             |
| ----------------------- | ---------------------------------------------------------- |
| **Environment**         | Vault path, dewey binary location                          |
| **Workspace**           | `.uf/dewey/` directory, config files, `sources.yaml`       |
| **Database**            | `graph.db` health, page/block/embedding counts             |
| **Sources in Database** | Per-source page counts                                     |
| **Embedding Layer**     | Ollama availability, model status, legacy model advisory   |
| **Synthesis Layer**     | Provider type (ollama/vertex/unconfigured), resolved endpoint, model name, connectivity status, and (for Ollama) model availability |
| **MCP Server**          | Lock file, `opencode.json` configuration                   |
| **Summary**             | Overall health with emoji markers (✓ pass, ⚠ warn, ✗ fail) |

```bash
dewey doctor
```

When `granite-embedding:30m` is configured, `dewey doctor` displays an informational advisory suggesting an upgrade to the Granite Embedding R2 model. This is not a warning or error — Dewey functions normally with the current model. A future Dewey release will update the default embedding model to Granite Embedding R2 once it is available on Ollama.

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

### Embedding Model Not Pulled

If Ollama is running but the configured embedding model has not been pulled, Dewey does not exit with a fatal error. Instead, it logs a warning and continues in **keyword-only mode** — structured graph queries, tag lookups, wikilink traversal, and keyword search all work normally. Only semantic (vector-based) search is unavailable until the model is pulled.

This means you can run `dewey serve` or `dewey index` immediately after installing Dewey, even before pulling the embedding model. Dewey is fully functional for structured queries; semantic search becomes available once you run `ollama pull granite-embedding:30m`.

## Curated Knowledge Stores

The `dewey curate` command synthesizes indexed content into structured knowledge articles, grouped by topic. Configure curation targets in `knowledge-stores.yaml`:

```yaml
stores:
  - tag: authentication
    description: "Authentication patterns and decisions"
  - tag: deployment
    description: "Deployment procedures and configuration"
```

Run `dewey curate` to process all configured stores, or `dewey curate --store authentication` to curate a single topic. Curated articles receive the `curated` trust tier — ranking above raw `draft` content but below `validated` and `authored` content in search results.

Background curation runs automatically during `dewey serve`, keeping curated articles current as new learnings and indexed content arrive. You can also trigger curation manually or from CI pipelines.

When using Vertex AI as the synthesis provider, note that curation of large vaults may take several minutes — Vertex AI supports up to 16000 max output tokens per request with a 300-second timeout for large prompt processing.

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
dewey promote --id gotcha-20260502T143022-alice
```

## Trust Tiers

Dewey classifies all stored knowledge into five trust tiers. Tiers affect how content ranks in search results and allow agents to filter by quality level.

| Tier           | Meaning                             | How Content Gets This Tier                                                    |
| -------------- | ----------------------------------- | ----------------------------------------------------------------------------- |
| **Authored**   | Human-written content               | Content created directly by humans (specs, READMEs, design docs)              |
| **Curated**    | Machine-synthesized, topic-grouped  | Articles produced by `dewey curate` from configured knowledge stores          |
| **Validated**  | Machine-generated, human-reviewed   | Content generated by agents and approved by a human via `dewey promote`       |
| **Draft**      | Machine-generated, not yet reviewed | Content stored by agents via `store_learning` or generated by `dewey compile` |
| **Untrusted**  | External or unverified content      | Content from sources configured with `trust_tier: untrusted` in `sources.yaml` |

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

The tool returns a `{tag}-{YYYYMMDDTHHMMSS}-{author}` identity string (e.g., `gotcha-20260502T143022-alice`), which uniquely identifies the stored learning for future reference. The author component comes from the `DEWEY_AUTHOR` environment variable (defaults to the system username).

> **Migration note**: Prior to v3.2.0, learning identities used a sequential format (`{tag}-{sequence}`, e.g., `gotcha-003`). Existing learnings in the old format are automatically re-ingested on startup — no manual migration is required. The old `tags` parameter is still accepted for backward compatibility.

### Search Result Metadata

When retrieving learnings via `dewey_semantic_search` or `dewey_semantic_search_filtered`, results include additional metadata fields:

| Field        | Description                                              |
| ------------ | -------------------------------------------------------- |
| `created_at` | Timestamp when the learning was stored                   |
| `category`   | The learning's classification (if provided at storage)   |
| `tier`       | The trust tier of the content (authored/curated/validated/draft/untrusted) |

Use the `tier` parameter on `dewey_semantic_search_filtered` to filter results by trust level — for example, restricting to `authored` or `validated` content when making critical decisions.

## Troubleshooting

Common issues and how to resolve them:

| Issue                  | Symptoms                               | Resolution                                                                                                                                  |
| ---------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| MCP server timeout     | OpenCode shows connection timeout      | Check `.gitignore` for large directories being indexed; run `dewey reindex`                                                                 |
| Ollama not running     | `dewey doctor` shows embedding layer ✗ | Run `ollama serve` or install Ollama (`brew install --cask ollama`)                                                                         |
| Model not pulled       | Semantic search returns no results; log shows "embedding model not available" | Run `ollama pull granite-embedding:30m`. Dewey continues in keyword-only mode until the model is available.                                 |
| Lock file conflicts    | "Another dewey instance is running"    | Only one `dewey serve` per vault; check for stale `.uf/dewey/.dewey.lock`                                                                   |
| Low embedding coverage | Semantic search returns few results    | Run `dewey index` to generate embeddings for new content                                                                                    |
| Slow startup           | First `dewey serve` takes minutes      | Normal for large repos on first index; subsequent startups are near-instant. Check `.gitignore` to exclude `node_modules/`, `vendor/`, etc. |

If `dewey doctor` shows failures, start by addressing the ✗ items — each diagnostic section includes enough context to identify the root cause.

## Next Steps

- Read the [Dewey tool page](/docs/team/dewey/) for architecture details, query capabilities, and how each hero uses Dewey
- See [Common Workflows](/docs/getting-started/common-workflows/) for how Dewey provides context during the hero lifecycle
- Explore the role-specific guides to see how [developers](/docs/getting-started/developer/), [testers](/docs/getting-started/tester/), [product owners](/docs/getting-started/product-owner/), and [product managers](/docs/getting-started/product-manager/) each use Dewey
