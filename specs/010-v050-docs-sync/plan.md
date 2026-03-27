# Implementation Plan: v0.5.0 Documentation Sync

**Branch**: `010-v050-docs-sync` | **Date**: 2026-03-26 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/010-v050-docs-sync/spec.md`

## Summary

Synchronize the Unbound Force website documentation with changes introduced in the upstream `unbound-force` repository between v0.4.0 and v0.5.0. This is a content-only change affecting Markdown files and one HTML template. Key updates: expanded `uf setup` documentation (16 steps), new workflow commands (`/workflow seed`, `/workflow start` flags, `.unbound-force/config.yaml`), corrected Divisor persona count (3 → 5), embedding model alignment documentation, branch convention documentation, and Dewey MCP tool count reconciliation.

## Technical Context

**Language/Version**: Hugo (Go-based SSG) via `@thulite` npm packages; Node.js >= 20.11.0; Go >= 1.23  
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`  
**Storage**: N/A (static Markdown files)  
**Testing**: Manual -- `npm run build` must succeed; visual verification in dev server  
**Target Platform**: GitHub Pages (static site)  
**Project Type**: Static documentation website  
**Performance Goals**: N/A (build-time only)  
**Constraints**: All content must follow Doks/Hugo conventions; Markdown frontmatter required; start body with H2  
**Scale/Scope**: ~10 Markdown files + 1 HTML template modified

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### I. Content Accuracy -- PASS

This feature exists specifically to bring website content into alignment with the v0.5.0 upstream repository. Every change is sourced from the actual v0.5.0 codebase (`setup.go`, `scaffold.go`, `unbound-force.md`, agent files, Dewey README). No features are fabricated or overstated.

### II. Minimal Footprint -- PASS

All changes are Markdown content updates and one HTML template update (Divisor description). No new dependencies, no custom CSS, no new layouts, no JavaScript. The Doks theme handles all rendering.

### III. Visitor Clarity -- PASS

Updates improve visitor clarity by:

- Making setup documentation match actual tool behavior (users get what the docs describe)
- Documenting commands users need to invoke new capabilities
- Resolving cross-page inconsistencies (Divisor persona count, Dewey tool count)
- Adding embedding alignment instructions users need for optimal operation

## Project Structure

### Documentation (this feature)

```text
specs/010-v050-docs-sync/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── quickstart.md        # Phase 1 output
├── checklists/
│   └── requirements.md  # Spec quality checklist
└── tasks.md             # Phase 2 output (created by /speckit.tasks)
```

### Source Code (repository root)

```text
content/docs/getting-started/
├── _index.md               # Minor -- no changes needed (already updated in spec 009)
├── common-workflows.md     # Major -- setup section, workflow commands, config, branch conventions
├── developer.md            # Medium -- uf setup description, branch conventions
├── product-owner.md        # Medium -- /workflow seed command
├── knowledge.md            # Medium -- embedding alignment env vars
├── tester.md               # No changes expected
└── product-manager.md      # No changes expected

content/docs/team/
├── _index.md               # Minor -- Divisor persona count fix
├── the-divisor.md          # Major -- 3 personas → 5 personas
└── dewey.md                # Minor -- MCP tool count reconciliation

content/docs/projects/
├── dewey.md                # Minor -- MCP tool count reconciliation
└── _index.md               # Minor -- no changes needed (already accurate)

layouts/
└── home.html               # No changes expected
```

**Structure Decision**: Content-only updates to existing Markdown files. No new files created in the content tree. No new directories. No layout or style changes.

## File Change Matrix

| File                                               | Priority | FRs Covered                                            | Change Summary                                                                                 |
| -------------------------------------------------- | -------- | ------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| `content/docs/getting-started/common-workflows.md` | P1       | FR-001, FR-002, FR-003, FR-004, FR-005, FR-006, FR-010 | Rewrite setup section; add workflow command docs; add config file docs; add branch conventions |
| `content/docs/getting-started/developer.md`        | P1       | FR-012, FR-010                                         | Update uf setup description; add branch convention note                                        |
| `content/docs/getting-started/product-owner.md`    | P1       | FR-013                                                 | Add `/workflow seed` command reference                                                         |
| `content/docs/getting-started/knowledge.md`        | P2       | FR-009                                                 | Add embedding alignment env vars section                                                       |
| `content/docs/team/the-divisor.md`                 | P2       | FR-007                                                 | Rewrite council section: 3 → 5 personas with descriptions                                      |
| `content/docs/team/_index.md`                      | P2       | FR-008                                                 | Fix Divisor description: "three" → "five" personas                                             |
| `content/docs/team/dewey.md`                       | P3       | FR-011                                                 | Fix MCP tool count to "40 tools across 10 categories"                                          |
| `content/docs/projects/dewey.md`                   | P3       | FR-011                                                 | Fix MCP tool count to "40 tools across 10 categories"                                          |

## Complexity Tracking

No constitution violations. No complexity tracking needed.
