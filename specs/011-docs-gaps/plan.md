# Implementation Plan: Documentation Gaps

**Branch**: `011-docs-gaps` | **Date**: 2026-03-29 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/011-docs-gaps/spec.md`

## Summary

Address 5 documentation gaps identified in the website audit to bring completeness from ~80% to ~95%. Create 2 new pages (constitution, artifacts) and expand 2 existing pages (common-workflows, developer) with workflow commands, convention pack documentation, and `uf init` standalone documentation.

## Technical Context

**Language/Version**: Hugo (Go-based SSG) via `@thulite` npm packages; Node.js >= 20.11.0; Go >= 1.23
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`
**Storage**: N/A (static Markdown files)
**Testing**: Manual -- `npm run build` must succeed; visual verification in dev server
**Target Platform**: GitHub Pages (static site)
**Project Type**: Static documentation website
**Performance Goals**: N/A (build-time only)
**Constraints**: All content must follow Doks/Hugo conventions; Markdown frontmatter required; start body with H2
**Scale/Scope**: 2 new Markdown pages + 2 modified Markdown pages + 1 navigation config file

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### I. Content Accuracy -- PASS

All content for new pages is sourced from authoritative upstream documents:

- Constitution page: org constitution v1.1.0 (`.specify/memory/constitution.md` in the unbound-force repo)
- Artifacts page: AGENTS.md "Inter-Hero Artifact Types" section and `specs/009-shared-data-model/`
- Workflow commands: `.opencode/command/workflow-status.md`, `workflow-list.md`, `workflow-advance.md`
- Convention packs: `.opencode/unbound/packs/` directory (6 files verified)
- `uf init`: `internal/scaffold/scaffold.go` (flags, file ownership, embedded assets)

No features are fabricated. Content will be adapted for a website audience per the Content Accuracy principle.

### II. Minimal Footprint -- PASS

All changes are Markdown content. No new CSS, no custom layouts, no JavaScript, no new dependencies. Navigation updates use the existing `menus.en.toml` configuration pattern. Convention packs and `uf init` documentation are sections within existing pages (not new standalone pages), per the principle of minimal fragmentation.

### III. Visitor Clarity -- PASS

New pages fill documented gaps in the information hierarchy. Constitution and artifacts pages address the two largest content holes. Both will be accessible within two clicks from the homepage via sidebar navigation. Cross-links to existing pages avoid duplication and maintain the logical flow: homepage -> getting started -> specific topics.

## Project Structure

### Documentation (this feature)

```text
specs/011-docs-gaps/
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
├── constitution.md          # NEW -- US1: 4 principles, governance, hero alignment
├── artifacts.md             # NEW -- US2: 7 artifact types, envelope format, lifecycle mapping
├── common-workflows.md      # MODIFIED -- US3: add /workflow status, list, advance
└── developer.md             # MODIFIED -- US4+US5: expand convention packs, add uf init section

config/_default/menus/
└── menus.en.toml            # MODIFIED -- add sidebar entries for constitution and artifacts
```

**Structure Decision**: New pages placed under `content/docs/getting-started/` to maintain the existing section structure. No new Architecture section created. Convention packs and `uf init` are sections within `developer.md`, not standalone pages.

## File Change Matrix

| File                                               | Priority | FRs Covered                            | Change Summary                                                                                        |
| -------------------------------------------------- | -------- | -------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `content/docs/getting-started/constitution.md`     | P1       | FR-001, FR-002, FR-003, FR-013, FR-014 | New page: 4 principles with MUST/SHOULD rules, governance, hero alignment, Constitution Check process |
| `content/docs/getting-started/artifacts.md`        | P1       | FR-004, FR-005, FR-006, FR-013, FR-014 | New page: envelope format, 7 artifact types with producer/consumer, lifecycle stage mapping           |
| `content/docs/getting-started/common-workflows.md` | P2       | FR-007, FR-008, FR-009                 | Add Workflow Management section: /workflow status, /workflow list, /workflow advance                  |
| `content/docs/getting-started/developer.md`        | P2+P3    | FR-010, FR-011                         | Expand convention packs section; add uf init standalone section                                       |
| `config/_default/menus/menus.en.toml`              | P1       | FR-012                                 | Add sidebar entries for constitution and artifacts pages                                              |

## Complexity Tracking

No constitution violations. No complexity tracking needed.
