## Why

The website contains factual errors and stale data that actively mislead users. Two are CRITICAL severity (users following the instructions will get wrong results), and two are HIGH severity (users get incorrect expectations about tooling).

These corrections address GitHub issues #53, #54, #55, and #56 — all tagged `release-ready`.

## What Changes

Four surgical text corrections across four documentation pages:

1. **`/finale` merge behavior** (#53): Four pages describe `/finale` as merging PRs. The shipped command explicitly states "NEVER merge the PR" — it creates PRs for review only. Fix across `common-workflows.md`, `quick-start.md`, `developer.md`, and `blog/unleash-in-practice.md`.

2. **`/unleash` branch support** (#54): `common-workflows.md` states `/unleash` rejects `opsx/*` branches. Since Spec 031, `/unleash` supports both `NNN-*` and `opsx/*` branches. Fix in `common-workflows.md`.

3. **`uf init` file count and breakdown** (#55): `developer.md` states "approximately 50 files" and lists stale command/agent counts. Correct to ~35 files, update the breakdown table to reflect externalized Speckit commands and added content personas.

4. **`mxf` install instruction** (#56): `product-manager.md` implies `mxf` is bundled with the `unbound-force` Homebrew package. It's a separate formula. Correct the install instruction.

## Capabilities

### New Capabilities
- None

### Modified Capabilities
- `docs/getting-started/common-workflows`: Corrected `/finale` and `/unleash` descriptions
- `docs/getting-started/quick-start`: Corrected `/finale` description (both code blocks)
- `docs/getting-started/developer`: Corrected `/finale` description (4 locations), file count, and command breakdown
- `docs/getting-started/product-manager`: Corrected `mxf` install instruction
- `blog/unleash-in-practice`: Corrected `/finale` description in "The Complete Loop" section

### Removed Capabilities
- None

## Impact

- 5 Markdown content pages modified (4 docs + 1 blog)
- No layout, style, or configuration changes
- No new pages created
- All changes are text corrections within existing content

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

This change exists specifically to fix Content Accuracy violations. All four corrections align documentation with the shipped command behavior (source of truth: `.opencode/command/*.md` in the `unbound-force/unbound-force` repo). The `uf init` file count (~35 files) and breakdown were verified against the current scaffold assets.

### II. Minimal Footprint

**Assessment**: PASS

Text-only corrections within existing Markdown content. No new pages, layouts, CSS, JavaScript, or dependencies.

### III. Visitor Clarity

**Assessment**: PASS

Correcting factual errors directly improves visitor clarity. Users reading corrected docs will get accurate expectations about `/finale` (creates PRs, does not merge), `/unleash` (supports both branch types), `uf init` (correct file count), and `mxf` (separate install).
