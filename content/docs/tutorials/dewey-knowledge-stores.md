---
title: "Setting Up Dewey Knowledge Stores"
description: "Step-by-step guide to configuring and using Dewey's curated knowledge stores — from configuration to semantic search with quality-scored, source-traced knowledge."
lead: "Configure knowledge stores, run automated curation, and search structured knowledge extracted from your indexed sources."
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 10
toc: true
---

## Prerequisites

Before configuring knowledge stores, verify that you have the following tools installed and running.

1. **Install Dewey v3.1.0 or later.** Knowledge stores require the curation pipeline introduced in v3.1.0.

    ```bash
    brew install unbound-force/tap/dewey
    dewey --version  # Confirm v3.1.0+
    ```

2. **Start Ollama and pull a generation model.** Dewey uses a local LLM to extract structured knowledge from your indexed sources. The default model is `llama3.2:3b`.

    ```bash
    ollama pull llama3.2:3b
    ollama serve  # Keep this running in a separate terminal
    ```

3. **Configure and index at least one content source.** Knowledge stores curate from indexed sources. You need a populated `.uf/dewey/sources.yaml` with at least one source that has been indexed.

    ```bash
    dewey index  # Index all configured sources
    ```

    If you have not configured any sources yet, see the [Dewey project documentation](/docs/projects/dewey/) for setup instructions before continuing.

## Creating a Knowledge Store

A knowledge store defines which indexed sources to curate and how to process them. Stores live in `.uf/dewey/knowledge-stores.yaml` at the root of your project.

### Step 1: Create the configuration file

Create or edit `.uf/dewey/knowledge-stores.yaml` in your project root:

```bash
mkdir -p .uf/dewey
touch .uf/dewey/knowledge-stores.yaml
```

### Step 2: Define a store

Each store has a `name`, a list of `sources` referencing source IDs from your `sources.yaml`, and optional settings that control curation behavior.

```yaml
stores:
  - name: team-knowledge
    description: "Curated knowledge from team docs and design decisions"
    sources:
      - github-design-docs
      - disk-meeting-notes
    settings:
      min_confidence: medium
      extract_decisions: true
      extract_patterns: true

  - name: api-reference
    description: "Structured API knowledge from code and docs"
    sources:
      - github-api-repo
      - web-api-docs
    settings:
      min_confidence: high
      extract_decisions: false
      extract_patterns: true
```

### Step 3: Understand the configuration fields

- **`name`** — A unique identifier for the store. Use lowercase with hyphens. This name appears in search filters and output directories.
- **`description`** — A human-readable summary of what this store contains. Dewey displays this in status output.
- **`sources`** — A list of source IDs that match entries in your `sources.yaml`. Dewey curates only from these sources when processing this store.
- **`settings.min_confidence`** — The minimum confidence level for extracted knowledge to be included. Options: `high`, `medium`, `low`, `flagged`. Defaults to `medium`.
- **`settings.extract_decisions`** — Whether to extract decision records from source content. Defaults to `true`.
- **`settings.extract_patterns`** — Whether to extract recurring patterns and conventions. Defaults to `true`.

## Running Curation

Curation is the process of extracting structured knowledge from your indexed sources. Dewey reads the raw content, identifies decisions, patterns, and key facts, then scores each extraction for confidence and quality.

### Full pipeline run

Run curation across all configured stores:

```bash
dewey curate
```

This processes every store defined in `knowledge-stores.yaml`. Dewey skips sources that have not changed since the last curation run.

### Single store curation

Target a specific store by name:

```bash
dewey curate --store team-knowledge
```

This is useful when you have added new sources to one store and want to curate only that store without reprocessing others.

### Force re-curation

Re-curate all sources regardless of whether they have changed:

```bash
dewey curate --force
```

Use this after upgrading Dewey, changing your generation model, or modifying store settings. The `--force` flag bypasses the change-detection cache and reprocesses every source.

### Examine output

Curated knowledge is stored in `.uf/dewey/knowledge/{store-name}/`. Each extraction is a Markdown file with frontmatter containing provenance metadata:

```bash
ls .uf/dewey/knowledge/team-knowledge/
```

```text
auth-design-decision-20260815.md
api-versioning-pattern-20260810.md
deployment-checklist-20260801.md
```

Each file contains the extracted knowledge, its confidence score, quality flags, and a link back to the original source document.

## Understanding Quality Flags

Dewey's curation pipeline assigns quality flags to extracted knowledge. These flags highlight areas where the extracted content may need human review or additional context.

### Flag types

| Flag | Meaning |
|------|---------|
| `missing_rationale` | A decision was extracted but no reasoning was stated in the source. The "why" is absent. |
| `implied_assumption` | The extraction depends on an assumption that is not explicitly stated in the source material. |
| `incongruent` | The extracted knowledge contradicts another piece of knowledge from a different source. |
| `unsupported_claim` | A factual claim was extracted but no supporting evidence or reference exists in the source. |

### Confidence levels

Every extraction receives a confidence score based on source quality, extraction clarity, and the presence or absence of quality flags:

- **`high`** — Clean extraction from a well-structured source. No quality flags. Ready for use without review.
- **`medium`** — Reasonable extraction with minor ambiguity. May have one non-critical flag. Review recommended but not required.
- **`low`** — Extraction has multiple flags or comes from an ambiguous source. Human review is strongly recommended before relying on this knowledge.
- **`flagged`** — Extraction has a critical flag such as `incongruent` or `unsupported_claim`. Do not use without human verification.

### Reviewing flagged content

List all flagged extractions across your stores:

```bash
dewey lint
```

The lint output includes a section for knowledge store quality. Address `incongruent` and `unsupported_claim` flags first — these indicate potential misinformation in your knowledge base.

## Searching Curated Knowledge

Once curation completes, curated knowledge is searchable through Dewey's semantic search with tier-based filtering.

### Search curated content only

Use the `tier` filter to restrict results to curated knowledge. Run this MCP tool call in your AI agent session:

```text
dewey_semantic_search_filtered(query: "authentication flow", tier: "curated")
```

This returns only knowledge that has been through the curation pipeline — extracted, scored, and quality-checked.

### Compare results across tiers

Dewey organizes content into trust tiers. Each tier represents a different level of processing and validation:

| Tier | Description | Use Case |
|------|-------------|----------|
| `authored` | Content you wrote directly (journal entries, manual notes) | Personal knowledge, session notes |
| `curated` | Machine-extracted and quality-scored knowledge from indexed sources | Structured team knowledge, decisions |
| `validated` | Curated knowledge that has been promoted after human review | High-trust reference material |
| `draft` | Raw learnings stored via `dewey_store_learning` before compilation | Work-in-progress insights |
| `untrusted` | Unverified external content | Background research, third-party docs |

Search a specific tier (MCP tool calls, run in your AI agent session):

```text
dewey_semantic_search_filtered(query: "deployment strategy", tier: "authored")
dewey_semantic_search_filtered(query: "deployment strategy", tier: "curated")
dewey_semantic_search_filtered(query: "deployment strategy", tier: "draft")
```

Comparing results across tiers reveals gaps. If a topic appears in `authored` notes but not in `curated` knowledge, the source documents may lack formal documentation for that topic.

## Configuring Background Curation

Dewey can run curation automatically so your knowledge stores stay current without manual intervention.

### Curation interval

Set the `curation_interval` field in your store configuration to control how often Dewey re-curates:

```yaml
stores:
  - name: team-knowledge
    sources:
      - github-design-docs
    settings:
      curation_interval: 10m  # Default: 10 minutes
```

Dewey checks for source changes at this interval. If no sources have changed, the curation cycle is a no-op. Set a longer interval (e.g., `1h`) for large source sets to reduce CPU usage.

### Curate on index

Enable `curate_on_index` to trigger curation immediately after a source is indexed:

```yaml
stores:
  - name: team-knowledge
    sources:
      - github-design-docs
    settings:
      curate_on_index: true
```

This ensures curated knowledge is always up to date with the latest indexed content. It adds latency to the indexing step but eliminates the delay between indexing and curation.

### Generation model

Dewey uses a local LLM for knowledge extraction. Set the model with the `DEWEY_GENERATION_MODEL` environment variable:

```bash
export DEWEY_GENERATION_MODEL=llama3.2:3b  # Default
```

Larger models produce higher-quality extractions but require more memory and processing time. The default `llama3.2:3b` balances quality and speed for most workloads. If you have the hardware, `llama3.1:8b` improves extraction accuracy for complex technical content.

## Monitoring with `dewey lint`

The `dewey lint` command includes knowledge store quality metrics alongside its other checks. Run it regularly to monitor the health of your curated knowledge.

```bash
dewey lint
```

### Knowledge store metrics

The lint output includes a dedicated section for each knowledge store:

```text
Knowledge Store: team-knowledge
  Total extractions: 47
  Confidence distribution:
    high:     28 (59.6%)
    medium:   12 (25.5%)
    low:       5 (10.6%)
    flagged:   2 (4.3%)
  Quality flags:
    missing_rationale: 3
    implied_assumption: 4
    incongruent: 1
    unsupported_claim: 1
  Uncompiled learnings: 6
```

### What to act on

1. **High `flagged` percentage** — If more than 10% of extractions are flagged, review your source quality. Poorly structured sources produce unreliable extractions.
2. **`incongruent` flags** — These indicate contradictions between sources. Resolve them by updating the outdated source or adding clarifying context.
3. **`unsupported_claim` flags** — These indicate claims without evidence. Either add supporting references to the source or remove the claim.
4. **Uncompiled learnings** — Run `dewey compile` to synthesize draft learnings into compiled knowledge articles.

## Storing Ad-Hoc Knowledge with `/dewey-store`

Not all knowledge comes from indexed sources. Use the `/dewey-store` command to capture insights, decisions, and patterns directly from conversations and ad-hoc observations.

### Fully specified mode

Provide the tag and category explicitly when you know exactly how to classify the knowledge:

```text
/dewey-store --tag auth-design --category decision
"We chose JWT over session cookies because our API serves both browser and CLI clients."
```

This creates a learning tagged with `auth-design` and categorized as a `decision`. Available categories: `decision`, `pattern`, `gotcha`, `context`, `reference`.

### Suggested mode

Let the agent propose a tag and category based on the content:

```text
/dewey-store
"The curation pipeline skips sources with fewer than 100 tokens to avoid noise."
```

The agent analyzes the content and suggests an appropriate tag (e.g., `curation-pipeline`) and category (e.g., `pattern`). You confirm or override before the learning is stored.

### Extract mode

Use `--extract` to process long threads and pull out the key insights:

```text
/dewey-store --extract
<paste long Slack thread or meeting notes here>
```

Extract mode identifies the most important decisions, patterns, and action items from the pasted content. Each extraction is stored as a separate learning with its own tag and category. This is useful for capturing knowledge from lengthy discussions without manually summarizing each point.

### Verifying stored knowledge

After storing ad-hoc knowledge, verify it appears in search results (MCP tool call, run in your AI agent session):

```text
dewey_semantic_search(query: "JWT authentication")
```

Stored learnings are immediately searchable. They start in the `draft` tier and move to `validated` after human review via `dewey promote`.

## Further Reading

- [Your AI Agent's Memory Survives Database Deletion](/blog/dewey-knowledge-stores/) — the design rationale behind file-backed learnings and curated knowledge stores
- [Dewey](/docs/projects/dewey/) — project overview, installation, and architecture
