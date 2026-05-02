## Why

The website contains factual errors and stale data that actively mislead users. Two are CRITICAL severity (users following the instructions will get wrong results), and two are HIGH severity (users get incorrect expectations about tooling).

These corrections address GitHub issues #53, #54, #55, and #56 — all tagged `release-ready`.

## What Changes

Four surgical text corrections across four documentation pages:

1. **`/finale` merge behavior** (#53): Three pages describe `/finale` as merging PRs. The shipped command explicitly states "NEVER merge the PR" — it creates PRs for review only. Fix across `common-workflows.md`, `quick-start.md`, and `developer.md`.

2. **`/unleash` branch support** (#54): `common-workflows.md` states `/unleash` rejects `opsx/*` branches. Since Spec 031, `/unleash` supports both `NNN-*` and `opsx/*` branches. Fix in `common-workflows.md`.

3. **`uf init` file count and breakdown** (#55): `developer.md` states "approximately 50 files" and lists stale command/agent counts. Correct to ~35 files, update the breakdown table to reflect externalized Speckit commands and added content personas.

4. **`mxf` install instruction** (#56): `product-manager.md` implies `mxf` is bundled with the `unbound-force` Homebrew package. It's a separate formula. Correct the install instruction.

## Capabilities

### New Capabilities
- None

### Modified Capabilities
- `docs/getting-started/common-workflows`: Corrected `/finale` and `/unleash` descriptions
- `docs/getting-started/quick-start`: Corrected `/finale` description
- `docs/getting-started/developer`: Corrected `/finale` description, file count, and command breakdown
- `docs/getting-started/product-manager`: Corrected `mxf` install instruction

### Removed Capabilities
- None

## Impact

- 4 Markdown content pages modified
- No layout, style, or configuration changes
- No new pages created
- All changes are text corrections within existing content

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

Documentation text corrections do not affect artifact-based communication between heroes.

### II. Composability First

**Assessment**: N/A

No tool behavior or dependency changes — text-only corrections.

### III. Observable Quality

**Assessment**: PASS

Corrections improve the accuracy of published documentation, which is a form of observable output quality. Users reading the docs will get correct information about command behavior.

### IV. Testability

**Assessment**: N/A

No code changes. Validation is manual: `npm run build` succeeds and corrected text renders properly.
