## Context

Dewey's `upgrade-embedding-model-r2` change ([dewey#53](https://github.com/unbound-force/dewey/issues/53)) adds a new environment variable (`DEWEY_CHUNK_MAX_CHARS`), a new config field (`embedding.max_chunk_chars`), and a `dewey doctor` advisory for the legacy `granite-embedding:30m` model. The website documentation must be updated to reflect these additions across three pages:

- `content/docs/projects/dewey.md` (project overview)
- `content/docs/getting-started/knowledge.md` (getting started guide)
- `content/docs/team/dewey.md` (team page)

The default embedding model remains `granite-embedding:30m` -- the R2 model is not yet published on Ollama. Documentation should note the R2 upgrade path without changing any installation commands.

## Goals / Non-Goals

### Goals
- Document `DEWEY_CHUNK_MAX_CHARS` env var with its default value (12288) and purpose
- Document `embedding.max_chunk_chars` config field in the YAML config example
- Document the `dewey doctor` legacy model advisory
- Mention the Granite Embedding R2 upgrade path as a future default
- Keep all existing content accurate -- additive changes only

### Non-Goals
- Changing the default model in installation commands (R2 is not yet on Ollama)
- Adding new pages or sections -- all changes fit within existing page structures
- Updating `ollama pull` commands (will be a follow-up when R2 is published)
- Documenting R2 model benchmarks or detailed technical comparisons

## Decisions

### D1: Update existing sections rather than creating new ones

The new configuration options (`DEWEY_CHUNK_MAX_CHARS`, `max_chunk_chars`) fit naturally into existing sections on each page. No new headings or page restructuring is needed.

**Rationale**: Follows the AGENTS.md Minimal Maintenance constraint -- prefer editing existing content over creating new structures.

### D2: Add env var to the Embedding Model Alignment section in knowledge.md

The `DEWEY_CHUNK_MAX_CHARS` env var is best documented alongside the existing `OLLAMA_MODEL` and `OLLAMA_EMBED_DIM` env vars in the Embedding Model Alignment section of the getting started guide.

**Rationale**: Users configuring embedding-related env vars will naturally look in this section. Keeps related configuration together.

### D3: Add config field to the existing YAML example in team/dewey.md

The `max_chunk_chars` field belongs in the `embedding:` config block that already exists on the team page's Embedding Model section.

**Rationale**: The config example already shows `provider`, `model`, and a comment about alternatives. Adding `max_chunk_chars` is a natural extension.

### D4: Document the doctor advisory in the existing doctor section

The legacy model advisory is an informational note, not a warning or error. It should be documented in the `dewey doctor` section of knowledge.md alongside the existing diagnostic table.

**Rationale**: Users who see the advisory in their terminal will search the doctor docs for explanation.

### D5: Forward-looking R2 language without urgency

Mention R2 as a future upgrade path in the Embedding Model sections, but do not present it as an action item. Use language like "a future release will update the default" rather than "you should upgrade."

**Rationale**: R2 is not yet on Ollama. Creating urgency for an unavailable model would confuse users.

## Risks / Trade-offs

### R1: Documentation may precede Dewey release

If the website updates deploy before the Dewey release containing `DEWEY_CHUNK_MAX_CHARS`, users may try to use a feature that does not exist in their installed version. This is low risk because the env var is optional and the docs will describe it as part of a specific Dewey version.

### R2: R2 model references may become stale

The R2 "future upgrade" language will need updating once R2 is published on Ollama. This is accepted -- the follow-up is already planned in the upstream Dewey issue.

### R3: Three pages to keep in sync

Dewey information is spread across project, getting-started, and team pages. Each serves a different audience, but they must stay consistent. The design mitigates this by making minimal, targeted edits to each page rather than duplicating content.
