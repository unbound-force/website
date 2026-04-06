## Why

Three GitHub issues (#20, #21, #22) report factual errors and missing content on the website caused by recent upstream changes in the unbound-force meta repo:

- **Bun was removed** from `uf setup` (Spec 024) but 3 pages still list it as a development tool
- **Hivemind was unified into Dewey** (Spec 022, PR #79) but 6 references to "Hivemind" remain across 3 pages
- **3 content agents were added** to The Divisor (PR #84) but the website still says "five canonical roles" and doesn't describe The Scribe, The Herald, or The Envoy
- **2 convention packs were added** (`content.md`, `content-custom.md`) but the developer guide still lists 7 packs instead of 9
- **Replicator missing** from the contributing page's repo list
- **Session Ritual section** still references `hive_ready()`/`hive_sync()` instead of the `/unleash` + `/finale` workflow
- **`.hive/` initialization** listed as a `uf setup` step when it's now handled by `replicator init` via `uf init`

These errors cause immediate confusion when a developer's experience doesn't match the documentation.

## What Changes

### Stale References Fixed

- Remove "Bun" from the development tools list on 3 pages
- Replace 6 "Hivemind" references with "Dewey" across 3 pages
- Fix `.hive/` initialization attribution (setup → init)
- Update Session Ritual to reference current workflow patterns

### New Content Added

- Document 3 content agents (The Scribe, The Herald, The Envoy) on the Divisor team page
- Update Divisor persona count from 5 to 8 (5 review + 3 content)
- Add `content.md` and `content-custom.md` to the convention packs table
- Add Replicator to the contributing page

## Capabilities

### New Capabilities

- Content agent documentation: visitors can learn about The Scribe, The Herald, and The Envoy on the Divisor team page

### Modified Capabilities

- Convention packs table: updated from 7 to 9 files
- Contributing page: includes Replicator repo for issue reporting
- Divisor persona count: reflects the 8 canonical roles (5 review + 3 content)

### Removed Capabilities

- Bun references: removed from setup documentation (no longer installed)
- Hivemind references: replaced with Dewey (unified memory layer)
- Stale Session Ritual: updated to current workflow

## Impact

**Affected files** (8):

- `content/docs/getting-started/quick-start.md` — remove Bun
- `content/docs/getting-started/developer.md` — remove Bun, update packs table, fix Session Ritual
- `content/docs/getting-started/common-workflows.md` — remove Bun, fix Hivemind, fix .hive/ attribution
- `content/blog/unleash-in-practice.md` — fix 3 Hivemind references
- `content/docs/team/the-divisor.md` — fix persona count, add content agents, fix Hivemind
- `content/docs/team/_index.md` — update Divisor summary
- `content/docs/contributing/_index.md` — add Replicator

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

Documentation-only change. No artifacts, protocols, or hero communication affected.

### II. Composability First

**Assessment**: N/A

No dependencies introduced or removed. Content pages are independent.

### III. Observable Quality

**Assessment**: PASS

Fixes factual inaccuracies (Bun, Hivemind, persona count) that undermined the website's accuracy as the public-facing documentation. Adds missing content (content agents, convention packs) that were shipped in the meta repo but undocumented.

### IV. Testability

**Assessment**: N/A

Static documentation site. Validation is `npm run build` + visual verification.
