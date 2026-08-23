---
title: "Your AI Agent's Memory Survives Database Deletion — Dewey v3.1.0 Knowledge Stores"
description: "Dewey v3.1.0 introduces curated knowledge stores — an automated pipeline that extracts structured knowledge from indexed sources using LLM analysis. Paired with file-backed learnings that survive database deletion."
lead: "Delete the database, restart Dewey, and every learning is back. File-backed persistence plus automated curation turn raw documents into structured, quality-scored knowledge."
slug: "dewey-knowledge-stores"
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 103
toc: true
categories: ["Engineering"]
tags: ["dewey", "knowledge-stores", "curation", "semantic-search"]
contributors: ["Unbound Force"]
---

## The Fragility Problem

AI agents accumulate knowledge across sessions. Decisions made during code review, patterns discovered while debugging, gotchas encountered during deployment — all of it feeds back into future work. In most agent frameworks, that knowledge lives in a single SQLite database. Delete it, corrupt it, or move to a new machine, and every learning is gone.

The loss of structured knowledge is only half the problem. Organizations generate vast amounts of unstructured content — meeting notes, Slack exports, GitHub discussions, design documents — that contain critical decisions and patterns buried in prose. No agent can extract those insights without reading every document, every time. The knowledge exists, but it's locked inside formats that resist automated extraction.

This is the third post in our Dewey blog arc. In [The Librarian vs The Index](/blog/dewey-vs-karpathy/), we explored why semantic search alone isn't enough. In [How Dewey Became a Knowledge Curator](/blog/dewey-curator/), we showed how Dewey moved beyond retrieval into active knowledge management. Now we tackle the two remaining gaps: durability and automated extraction.

## Two Complementary Solutions

Dewey v3.1.0 addresses both problems with a pair of features that work together but solve distinct failure modes.

### File-Backed Learnings

Every `store_learning` call now dual-writes. The learning goes into SQLite for fast semantic search, and simultaneously to a plain markdown file at `.uf/dewey/learnings/{tag}-{seq}.md`. These files are plain text, human-readable, and version-controllable with git.

Delete the database. Restart Dewey. On startup, the file-backed persistence layer scans the learnings directory and re-ingests every file. Embeddings are regenerated, metadata is restored, and semantic search works as if nothing happened. The database becomes a cache, not the source of truth.

This design means agent knowledge travels with the repository. Clone the repo on a new machine, start Dewey, and the full knowledge base is available. No database migration, no backup restoration, no cloud sync.

### Curated Knowledge Stores

File-backed learnings solve durability for knowledge that agents create. But what about knowledge that already exists in documents no agent wrote? Meeting notes that record an architectural decision. A GitHub discussion that resolves a design trade-off. A Slack thread where the team agreed on a naming convention.

Knowledge stores automate the extraction of structured knowledge from these raw sources. You configure a `.uf/dewey/knowledge-stores.yaml` that maps indexed sources to named stores, then run `dewey curate`. An LLM reads each document and extracts decisions, patterns, and facts — each tagged with source traceability, confidence scoring, and quality flags.

The LLM here is a structured extraction tool, not an "intelligent curator." It follows explicit extraction rules defined in the store configuration. It scores its own confidence. It flags ambiguity. The output is deterministic enough to diff, review, and version-control.

## The Workflow

### Step 1: Configure Knowledge Stores

A `knowledge-stores.yaml` file defines which indexed sources feed into which stores, and what extraction rules apply:

```yaml
stores:
  - name: architecture-decisions
    description: "Architectural decisions from design docs and discussions"
    sources:
      - github-discussions
      - design-docs
    settings:
      min_confidence: high
      extract_decisions: true
      extract_patterns: true

  - name: operational-runbooks
    description: "Operational patterns from incident reports and postmortems"
    sources:
      - incident-reports
    settings:
      min_confidence: medium
      extract_decisions: false
      extract_patterns: true
```

Each store targets a specific knowledge domain. The `sources` field references source IDs from your `sources.yaml` — the same sources you configure for Dewey's index. The `settings` block controls extraction behavior and the minimum confidence threshold for inclusion.

### Step 2: Run Curation

```bash
dewey curate
```

Dewey processes each configured store. For every source document, the LLM extracts structured knowledge items. Incremental curation (`dewey curate --incremental`) processes only documents that changed since the last run, keeping curation fast for large knowledge bases.

Background curation can be configured to run automatically when new content is indexed, keeping knowledge stores current without manual intervention.

### Step 3: Review Curated Knowledge

Each extracted knowledge item is written as a markdown file with YAML frontmatter that captures provenance and quality metadata:

```yaml
---
tag: architecture-decisions
category: decision
confidence: 0.85
quality_flags:
  - explicit_decision
  - multiple_sources
sources:
  - id: github-discussions/142
    title: "RFC: Event sourcing for audit trail"
    excerpt: "After evaluating CQRS vs traditional CRUD, the team decided..."
  - id: design-docs/adr-007.md
    title: "ADR-007: Event Sourcing for Compliance"
    excerpt: "Decision: Adopt event sourcing for all compliance-related..."
tier: curated
---

The audit trail uses event sourcing rather than CRUD operations.
This decision was driven by compliance requirements that mandate
a complete, immutable history of all state changes. The team
evaluated CQRS as an alternative but rejected it due to the
additional operational complexity of maintaining separate read
and write models.
```

Every curated fact traces back to its source documents. The `confidence` score reflects how explicitly the source stated the knowledge — a direct "we decided X" scores higher than an implied preference. Quality flags indicate whether the extraction found corroboration across multiple sources, whether the decision was explicitly stated, or whether the LLM flagged ambiguity.

### Step 4: File-Backed Recovery

The durability guarantee applies to both agent-created learnings and curated knowledge. Here's the recovery scenario:

```bash
# Store learnings during normal operation
# (via MCP tool calls from any agent)
dewey store-learning --tag auth "OAuth2 PKCE flow required for CLI tools"
dewey store-learning --tag deploy "Blue-green deploys need 30s drain period"

# Verify learnings exist
ls .uf/dewey/learnings/
# auth-1.md  deploy-1.md

# Disaster: database deleted
rm .uf/dewey/dewey.db

# Restart Dewey
dewey serve

# All learnings are automatically re-ingested from files
dewey search "OAuth2 CLI"
# Returns: "OAuth2 PKCE flow required for CLI tools" (confidence: 1.0)
```

No manual intervention. No restore-from-backup workflow. The files are the backup, and they're always in sync because they're written at the same time as the database entry.

## How This Compares

The combination of file-backed persistence and curated extraction differs from typical RAG (Retrieval-Augmented Generation) pipelines in several dimensions:

| Dimension | Typical RAG | Dewey v3.1.0 |
|-----------|-------------|--------------|
| Knowledge quality | Retrieves fragments without quality assessment | Extracts with confidence scoring and quality flags |
| Provenance | Chunk ID, sometimes source file | Full source traceability to specific documents and excerpts |
| Trust model | All retrieved content treated equally | 5-tier trust system (authored → validated → curated → draft → untrusted) |
| Data locality | Usually cloud-hosted vector DB | Local-only (SQLite + Ollama) |
| Durability | Database is the single source of truth | File-backed dual-write survives database deletion |
| Portability | Tied to the vector DB instance | Git-compatible files travel with the repository |

RAG pipelines optimize for retrieval speed. Dewey optimizes for knowledge quality. A RAG system returns the most similar chunks to your query. Dewey returns knowledge items that have been extracted, scored, traced to sources, and assigned a trust tier. The difference matters when agents make decisions based on retrieved context — confidence scoring and provenance let the agent weigh evidence rather than treating all retrieved content as equally authoritative.

## What This Enables

File-backed learnings and curated knowledge stores unlock workflows that weren't practical before.

**Team knowledge sharing.** Because learnings are plain markdown files in a git-tracked directory, teams can share knowledge across machines by pushing and pulling the `.uf/dewey/learnings/` directory. A pattern discovered by one developer's agent is available to every team member's agent after a `git pull`.

**Knowledge auditing.** Every curated fact has a source trail. When a decision is questioned six months later, the provenance metadata points back to the original discussion, RFC, or meeting notes. No more "we decided this at some point but nobody remembers why."

**Incremental refinement.** Curated knowledge items can be promoted from `draft` to `curated` to `validated` tier as humans review and confirm them. The trust tier affects how agents weight the knowledge during retrieval — validated knowledge ranks higher than draft extractions.

## Try It

Dewey v3.1.0 is available now. To set up knowledge stores in your project, follow the companion tutorial: [Setting Up Knowledge Stores](/docs/tutorials/dewey-knowledge-stores/).

The tutorial walks through configuring your first knowledge store, running curation against your project's documentation, and verifying the file-backed recovery guarantee. Start with a single store targeting your project's design documents or ADRs — that's where the highest-value knowledge tends to live.

To install or upgrade Dewey:

```bash
go install github.com/unbound-force/dewey/cmd/dewey@v3.1.0
```

Read the [v3.1.0 release notes](https://github.com/unbound-force/dewey/releases/tag/v3.1.0) for the full changelog, and check out the [Dewey documentation](/docs/projects/dewey/) for the complete feature reference.
