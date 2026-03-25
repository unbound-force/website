---
title: "Dewey"
description: "Dewey is the semantic knowledge layer for the Unbound Force swarm — structured graph traversal plus vector-based semantic search for AI agent context."
lead: "The semantic knowledge layer that gives every hero cross-repository context."
date: 2026-03-25T00:00:00+00:00
draft: false
weight: 70
toc: true
---

## What Dewey Does

Dewey is a per-repository MCP server that provides AI agent personas with unified knowledge retrieval. It combines two complementary search modes into a single service:

- **Structured queries** — keyword search, tag lookup, wikilink traversal, and property queries against a knowledge graph built from your repository's Markdown files
- **Semantic queries** — vector-based similarity search that finds conceptually related content even when different terminology is used

Together, these modes let agents start with a vague concept ("authentication timeout issues") and progressively refine their understanding through structured navigation — discovering related specifications, past decisions, and connected documentation they did not know to ask for.

Dewey runs locally as an MCP server alongside your AI coding environment. It persists its indexes to a SQLite database (`.dewey/graph.db`), so subsequent sessions load near-instantly from the persistent index instead of rebuilding from scratch. No data leaves your machine — the embedding model runs locally via Ollama.

### The graphthulhu Relationship

Dewey is a hard fork of [graphthulhu](https://github.com/skridlevsky/graphthulhu), an Obsidian-compatible MCP knowledge graph server created by Max Skridlevsky. Dewey preserves every MCP tool that graphthulhu exposes and adds:

- **Persistent storage** — indexes survive across sessions instead of rebuilding from scratch
- **Semantic search** — vector embeddings via locally-run Ollama models
- **Pluggable content sources** — GitHub API, web crawl, and local disk (graphthulhu only indexes local Markdown)
- **Incremental updates** — only changed files are re-indexed, not the entire corpus

Existing agent configurations that use graphthulhu can migrate to Dewey by changing the MCP server name in their configuration. No agent prompt changes are required for existing functionality.

## Query Capabilities

Dewey exposes 40+ MCP tools across 10 categories. For knowledge retrieval, agents primarily use 9 query tools across two search modes.

### Structured Queries

Inherited from graphthulhu, these tools provide precise, deterministic retrieval:

| Tool               | What It Does                                                                      |
| ------------------ | --------------------------------------------------------------------------------- |
| `search`           | Full-text keyword search across all indexed content                               |
| `find_by_tag`      | Find pages by tag values with child tag hierarchy support                         |
| `query_properties` | Structured queries against YAML frontmatter properties (eq, contains, gt, lt)     |
| `get_page`         | Retrieve the full block tree of a specific document                               |
| `traverse`         | Follow wikilinks to discover relationships between documents via BFS path-finding |
| `find_connections` | Discover direct links, shortest paths, and shared connections between pages       |

### Semantic Queries

New in Dewey, these tools use vector embeddings for conceptual similarity:

| Tool                             | What It Does                                                                                 |
| -------------------------------- | -------------------------------------------------------------------------------------------- |
| `dewey_semantic_search`          | Find documents semantically similar to a natural language query, ranked by cosine similarity |
| `dewey_similar`                  | Given a document ID, find the most similar documents in the index                            |
| `dewey_semantic_search_filtered` | Semantic search constrained by source type, repository, or property values                   |

### Combined Queries

The most powerful retrieval pattern combines both modes:

1. **Semantic search** to discover broadly relevant content: "authentication timeout issues"
2. **Property filter** to narrow by source: `source = "github"` AND `repo = "gaze"`
3. **Graph traversal** to explore relationships: follow wikilinks from the discovered documents to find connected specifications

This pattern enables agents to start with a vague concept and progressively refine their understanding through structured navigation.

## Content Sources

Dewey indexes content from three pluggable source types. Each source implements a common interface, so adding new source types (Confluence, Notion, S3) is a matter of implementing the interface — not modifying core indexing logic.

### Local Disk

The foundational source, inherited from graphthulhu. Indexes all Markdown files in the repository, including hidden directories (`.opencode/`, `.specify/`, `.muti-mind/`). Files are watched for changes in real time via fsnotify — when you save a file, Dewey re-indexes only the changed content using SHA-256 content hashing for change detection.

### GitHub API

Fetches issues, pull requests, READMEs, and documentation directories from whitelisted repositories in your GitHub organization. This provides cross-repository context that is impossible with a single-repo knowledge graph. Authentication uses the `gh` CLI's existing credentials.

### Web Crawl

Fetches and indexes documentation from toolstack websites — Go standard library docs, framework documentation, tool references. Crawls respect `robots.txt`, impose configurable rate limiting, and cache content locally. HTML is converted to Markdown for consistent indexing.

See the [getting-started guide](/docs/getting-started/knowledge/#source-configuration) for YAML configuration examples for each source type.

## Embedding Model

Dewey uses [IBM Granite Embedding](https://www.ibm.com/granite/docs/models/embedding/) for vector embeddings — a 63 MB model that runs locally via Ollama.

| Attribute           | Value                                                       |
| ------------------- | ----------------------------------------------------------- |
| **Developer**       | IBM Research                                                |
| **License**         | Apache 2.0                                                  |
| **Training data**   | Permissibly licensed public datasets with full transparency |
| **Model size**      | 30M parameters (63 MB)                                      |
| **Context window**  | 512 tokens                                                  |
| **Load time**       | 1-2 seconds on Apple Silicon                                |
| **Embedding speed** | ~10-50 ms per chunk                                         |

Enterprise licensing provenance matters. Granite's training data is fully disclosed and permissibly licensed — there are no questions about whether the model was trained on proprietary or restricted content. This is a deliberate choice for organizations where licensing provenance is a compliance requirement.

The embedding model is configurable. While Granite is the recommended default, teams can swap to any Ollama-compatible embedding model by editing `.dewey/config.yaml`:

```yaml
embedding:
  provider: ollama
  model: granite-embedding:30m
  # Alternative: granite-embedding:278m for multilingual content
```

## How Heroes Use Dewey

Every hero in the swarm benefits from Dewey, but in different ways. Dewey is always optional — all heroes function without it, falling back to direct file reads and CLI queries.

| Hero                                              | Role             | How They Use Dewey                                                                             |
| ------------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------- |
| [Muti-Mind](/docs/getting-started/product-owner/) | Product Owner    | Cross-repo issue discovery, past acceptance criteria, backlog pattern analysis                 |
| [Cobalt-Crush](/docs/getting-started/developer/)  | Developer        | Toolstack API documentation, implementation patterns from other repos, related spec artifacts  |
| [Gaze](/docs/getting-started/tester/)             | Tester           | Quality patterns across repos, CRAP score baselines, known failure modes for similar code      |
| [The Divisor](/docs/team/the-divisor/)            | Reviewer Council | Convention pack context enriched with framework docs, recurring review findings across the org |
| [Mx F](/docs/getting-started/product-manager/)    | Manager          | Cross-repo velocity trends, retrospective outcomes, coaching pattern discovery                 |

For installation and configuration, see the [Dewey getting-started guide](/docs/getting-started/knowledge/).
