## Why

GitHub issue [#211](https://github.com/unbound-force/website/issues/211) tracks an upstream behavioral change in `unbound-force/unbound-force#321`: `uf init --force` now passes `--no-embeddings` to `dewey index`, deferring embedding generation. This eliminates the indefinite hang that occurred on large workspaces (~2300 pages, 40 sources) when Ollama embedding generation ran synchronously during init.

The website currently documents the pre-fix behavior in at least 5 locations, creating a Constitution Principle I (Content Accuracy) violation — the docs describe behavior that no longer matches the upstream implementation.

## What Changes

Update all website pages that describe `uf init` + Dewey indexing behavior to reflect the new `--no-embeddings` default during init. Add guidance on how users can trigger embedding generation separately after initialization.

## Capabilities

### New Capabilities
- None (documentation-only change)

### Modified Capabilities
- `uf init` documentation: Updated to reflect that `dewey index` now runs with `--no-embeddings` during init, deferring embedding generation for faster, predictable initialization
- Post-init embedding workflow: New guidance section explaining how to trigger embedding generation separately via `dewey index` (without `--no-embeddings`)

### Removed Capabilities
- None

## Impact

**Affected pages** (identified during Divisor panel triage):

| Page | Location | What changes |
|------|----------|-------------|
| `content/docs/reference/cli.md` | Line ~41 | `--force` flag description — add note about `--no-embeddings` default |
| `content/docs/getting-started/developer.md` | Lines ~291, ~322 | Sub-Tool Initialization section — update `--force` re-index description |
| `content/docs/getting-started/knowledge.md` | Lines ~107–111 | `uf init` three-step sequence — note step 3 now uses `--no-embeddings` |
| `content/docs/getting-started/common-workflows.md` | Line ~436 | Setup workflow — update `dewey index` mention |
| `content/blog/dewey-knowledge-retrieval.md` | Line ~98 | `uf init` mention — add `--no-embeddings` caveat |

**Not affected**: The `--no-embeddings` flag itself is already documented in the Dewey Global CLI Flags table (`knowledge.md` ~line 438). The gap is that `uf init` now uses this flag by default.

**No release notes page**: The website does not currently have a changelog or release notes section. This change will skip release notes (out of scope for a documentation sync fix).

## Constitution Alignment

Assessed against the Unbound Force org constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

This change directly serves Principle I. The current docs describe `uf init` as running `dewey index` without qualification, but the upstream implementation now passes `--no-embeddings`. Updating the docs to reflect the actual behavior is a textbook Content Accuracy obligation. The constitution states: "When upstream repository content changes, the corresponding website pages MUST be reviewed and updated to stay in sync."

### II. Minimal Footprint

**Assessment**: PASS

All changes are Markdown-only content edits to existing pages. No new custom HTML, CSS, JavaScript, templates, or dependencies are introduced. This is the simplest possible implementation.

### III. Visitor Clarity

**Assessment**: PASS

Without this update, users who read that `uf init --force` re-indexes Dewey would expect semantic search to work immediately after init, then be confused when it doesn't (because embeddings were not generated). The updated docs will set correct expectations and provide a clear path for users who want embeddings: run `dewey index` separately.
