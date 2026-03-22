# Implementation Plan: Binary Rename -- Website Updates

**Branch**: `006-binary-rename` | **Date**: 2026-03-22 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/006-binary-rename/spec.md`

## Summary

Rename all CLI binary references across the website from `unbound` to `unbound-force` (canonical) / `uf` (preferred short alias). This addresses the naming collision with the NLnet Labs Unbound DNS resolver. The change is strictly content-level: 3 Markdown files contain ~14 CLI reference locations that need updating. No layout, config, SCSS, or structural changes are required.

## Technical Context

**Language/Version**: Hugo (Go-based SSG) via `@thulite` npm packages; Node.js >= 20.11.0; Go >= 1.23
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`
**Storage**: N/A (static Markdown files)
**Testing**: Manual -- `npm run build` must succeed; visual verification via `npm run dev`
**Target Platform**: GitHub Pages (static HTML)
**Project Type**: Static documentation site (Hugo + Doks theme)
**Performance Goals**: N/A (content-only change, no performance implications)
**Constraints**: Must not break the build; must not alter non-CLI references to "Unbound Force" organization name
**Scale/Scope**: 3 files, ~14 individual text replacements

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### I. Content Accuracy -- PASS

The entire purpose of this feature is to improve content accuracy. The current site references the `unbound` CLI binary, which will be renamed upstream (Spec 013 in the meta repo). Updating the website to reflect the actual binary name (`unbound-force` / `uf`) directly serves this principle. After implementation, every CLI command on the site will match the actual installed binary.

### II. Minimal Footprint -- PASS

This change modifies only Markdown content. No custom HTML, CSS, JavaScript, or template overrides are introduced. No new dependencies are added. The implementation is the simplest possible approach: find the old CLI references and replace them with the new ones, plus a brief explanatory note about the alias on each page.

### III. Visitor Clarity -- PASS

The rename improves visitor clarity by eliminating the naming collision with the NLnet Labs Unbound DNS resolver. Developers who have the DNS resolver installed will no longer be confused by commands that could invoke the wrong binary. The addition of a brief note explaining that `uf` is a short alias for `unbound-force` ensures visitors understand both names immediately.

**Gate result: ALL PASS -- proceeding to Phase 0.**

## Project Structure

### Documentation (this feature)

```text
specs/006-binary-rename/
├── spec.md              # Feature specification
├── plan.md              # This file
├── research.md          # Phase 0 output
├── checklists/
│   └── requirements.md  # Spec quality checklist
└── tasks.md             # Phase 2 output (/speckit.tasks command)
```

### Source Code (repository root)

```text
content/docs/getting-started/
├── developer.md         # 8 CLI references to update
├── common-workflows.md  # 4 CLI references to update
└── product-manager.md   # 2 CLI references to update
```

**Structure Decision**: No new files or directories are created. This is a pure content update to 3 existing Markdown files. No data-model.md, contracts/, or quickstart.md are needed since there are no data entities, external interfaces, or new setup procedures involved.

## Change Inventory

The following table maps every CLI reference that must be updated, organized by file and line number (based on current content). This serves as the authoritative change manifest for implementation.

### developer.md (8 references)

| Line | Current Text                                        | Replacement                                                          | FR             |
| ---- | --------------------------------------------------- | -------------------------------------------------------------------- | -------------- |
| 13   | ``Install the `unbound` CLI, then run setup``       | ``Install the `uf` CLI (short for `unbound-force`), then run setup`` | FR-002, FR-003 |
| 16   | `brew install unbound-force/tap/unbound`            | `brew install unbound-force/tap/unbound-force`                       | FR-001         |
| 17   | `unbound setup`                                     | `uf setup`                                                           | FR-002         |
| 20   | `` `unbound setup` detects your existing``          | `` `uf setup` detects your existing``                                | FR-002         |
| 30   | `unbound doctor`                                    | `uf doctor`                                                          | FR-002         |
| 39   | ``Run `unbound doctor` to verify``                  | ``Run `uf doctor` to verify``                                        | FR-002         |
| 138  | ``updated by `unbound init` ``                      | ``updated by `uf init` ``                                            | FR-002         |
| 163  | ``Run `unbound doctor` to verify your environment`` | ``Run `uf doctor` to verify your environment``                       | FR-002         |

### common-workflows.md (4 references)

| Line | Current Text                             | Replacement                                    | FR     |
| ---- | ---------------------------------------- | ---------------------------------------------- | ------ |
| 194  | `brew install unbound-force/tap/unbound` | `brew install unbound-force/tap/unbound-force` | FR-001 |
| 200  | `unbound setup`                          | `uf setup`                                     | FR-002 |
| 211  | ``Runs `unbound init` to scaffold``      | ``Runs `uf init` to scaffold``                 | FR-002 |
| 218  | `unbound doctor`                         | `uf doctor`                                    | FR-002 |

Additionally, the first CLI reference in the Environment Setup section (around line 191-194) should include a brief note that `uf` is the short alias for `unbound-force` (FR-003).

### product-manager.md (2 references)

| Line | Current Text                             | Replacement                                                  | FR             |
| ---- | ---------------------------------------- | ------------------------------------------------------------ | -------------- |
| 31   | ``included with the `unbound` package``  | ``included with the `unbound-force` package (the `uf` CLI)`` | FR-002, FR-003 |
| 34   | `brew install unbound-force/tap/unbound` | `brew install unbound-force/tap/unbound-force`               | FR-001         |

## Verification Strategy

1. **Build check**: Run `npm run build` after all edits. Must complete with zero errors.
2. **Content grep**: Search the built `public/` output for any remaining `unbound` CLI patterns (excluding org name references) to verify SC-001.
3. **Visual check**: Run `npm run dev` and manually verify each of the 3 affected pages renders correctly with the updated commands.
4. **Dark mode check**: Verify all 3 pages in dark mode to confirm inline code formatting renders correctly.
