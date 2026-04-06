## Context

The website has 3 open issues (#20, #21, #22) reporting stale references and missing content caused by upstream meta repo changes (Specs 022, 024; PRs #79, #83, #84). All changes are text edits to existing Markdown pages — no new pages, layouts, or CSS.

Per the proposal's constitution alignment: this is a documentation accuracy fix (Observable Quality: PASS). No hero communication, composability, or testability concerns.

## Goals / Non-Goals

### Goals

- Fix all factual errors identified in issues #20, #21, #22
- Document the 3 new content agents (Scribe, Herald, Envoy)
- Update convention pack count from 7 to 9
- Add Replicator to contributing page
- Close all 3 issues upon merge

### Non-Goals

- Rewriting the Divisor team page (only add content agent section and fix count)
- Updating the `/unleash` command file or internal agent files (separate scope)
- Adding content agent pages as standalone team pages (they are Divisor sub-personas, not independent heroes)

## Decisions

### 1. Content agents as a section within the Divisor page, not standalone pages

The 3 content agents (Scribe, Herald, Envoy) are Divisor sub-personas, same as Guard, Architect, and Adversary. They belong on the existing `team/the-divisor.md` page in a new "Content Creation Personas" section, following the same subsection pattern as the existing review personas.

### 2. Session Ritual: consolidate with Daily Workflow

The Session Ritual section in `developer.md` references `hive_ready()`/`hive_sync()` — MCP tool calls that are now internal to the Replicator coordination layer. The Daily Workflow section already describes the specify → unleash → finale loop. The Session Ritual section will be updated to reference `/finale` as the end-of-session pattern and removed the stale `hive_ready()`/`hive_sync()` references.

### 3. Convention pack table: add 2 rows, update any count references

The `content.md` and `content-custom.md` packs follow the same tool-owned/user-owned pattern as the existing packs. They are always-deployed (like `default.md` and `severity.md`), not language-gated.

### 4. ".hive/ initialization" attribution

The `.hive/` directory is now created by `replicator init` (called by `uf init`), not by `uf setup` directly. The setup tool list in `common-workflows.md` will be updated to remove the `.hive/` initialization mention from the Development tools bullet and let the existing `uf init` description handle it.

## Risks / Trade-offs

**Risk**: The Divisor persona count (5 → 8) appears on multiple pages. A search for "five personas" must catch all occurrences to avoid cross-page inconsistency.

**Mitigation**: Grep for "five personas", "five sub-personas", "five canonical", "council of five" across all content before marking complete.

**Trade-off**: The Session Ritual section could be fully removed (the Daily Workflow section covers the same ground). Keeping a simplified version maintains backward compatibility for readers who bookmarked it. Decision: simplify rather than remove.
