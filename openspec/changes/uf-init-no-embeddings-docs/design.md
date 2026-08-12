## Context

Upstream PR `unbound-force/unbound-force#321` changed `uf init --force` to pass `--no-embeddings` to `dewey index`. This eliminates an indefinite hang on large workspaces where Ollama embedding generation (IBM Granite `granite-embedding:30m`) could block init for minutes or longer.

The website currently documents the pre-fix behavior in 5 pages. The `--no-embeddings` flag itself is already documented in the Dewey Global CLI Flags table (`knowledge.md` ~line 438), so the gap is specifically about how `uf init` now uses this flag by default.

## Goals / Non-Goals

### Goals
- Update all pages that describe `uf init` + `dewey index` behavior to reflect the `--no-embeddings` default
- Add post-init embedding generation guidance where contextually appropriate (knowledge.md)
- Maintain cross-page consistency — all mentions of `uf init` Dewey indexing use the same language
- Preserve the existing `--no-embeddings` flag documentation in the Global CLI Flags table (no duplication)

### Non-Goals
- Creating a changelog or release notes page (no existing infrastructure; out of scope)
- Documenting the upstream PR implementation details (internal to `unbound-force/unbound-force`)
- Adding new sections or restructuring existing pages (in-place edits only)
- Documenting `--with-embeddings` opt-in during init (no such flag exists upstream)

## Decisions

### D1: In-place edits, no new sections

**Decision**: Modify existing text in-place rather than adding new documentation sections.

**Rationale**: The change is a behavioral modifier to an already-documented feature. Adding new sections would violate Constitution Principle II (Minimal Footprint). The only exception is a brief "tip" or callout in knowledge.md explaining how to generate embeddings after init, which fits naturally within the existing "What `uf init` Creates" section.

### D2: Reference existing `--no-embeddings` flag docs instead of duplicating

**Decision**: When mentioning `--no-embeddings` in the `uf init` context, link to or reference the existing Global CLI Flags table rather than re-explaining the flag.

**Rationale**: The flag is already documented in knowledge.md (~line 438). Duplicating the explanation creates a maintenance burden. A cross-reference keeps the content DRY.

### D3: Skip release notes

**Decision**: Do not create a release notes entry for this change.

**Rationale**: The website has no existing changelog or release notes infrastructure. Creating one is a larger effort that should be a separate change if desired. The issue's "release notes" item is acknowledged but deferred.

### D4: Blog post receives minimal update

**Decision**: Update the blog post mention (`dewey-knowledge-retrieval.md` ~line 98) only if the current text makes a factually incorrect claim. If the mention is general enough to remain accurate, leave it unchanged.

**Rationale**: Blog posts are point-in-time content. Over-updating them creates noise. The correction is only warranted if the text would mislead a reader.

## Risks / Trade-offs

### Risk: Upstream PR not yet merged
**Mitigation**: Verify `unbound-force/unbound-force#321` is merged before implementing. If not merged, defer implementation until it lands. Content Accuracy (Principle I) requires deriving from actual repository state, not planned changes.

### Trade-off: No release notes
**Accepted**: Users who look for a changelog won't find this change documented there. This is acceptable because the affected pages themselves will be updated, and there is no existing changelog mechanism to maintain.

### Risk: Incomplete page coverage
**Mitigation**: During implementation, run a grep for `uf init` and `dewey index` across all content to catch any pages not identified during triage. The 5 pages identified are a floor, not a ceiling.
