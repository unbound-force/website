# Implementation Plan: Dewey v3.0.0 Documentation Sync

**Branch**: `020-dewey-v3-docs-sync` | **Date**: 2026-04-11 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/020-dewey-v3-docs-sync/spec.md`

## Summary

Synchronize the Unbound Force website documentation with changes introduced in Dewey v3.0.0 (upstream PR #42, spec 013-knowledge-compile). This is a content-only change affecting three existing Markdown files. Key updates: tool count 40 → 48, source types 3 → 4 (code/Go AST added), new knowledge lifecycle features (compile, lint, promote), trust tier documentation, updated `store_learning` API parameters, and web crawler reliability note.

## Technical Context

**Language/Version**: Hugo (Go-based SSG) via `@thulite` npm packages; Node.js >= 20.11.0; Go >= 1.23  
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`  
**Storage**: N/A (static Markdown files)  
**Testing**: Manual — `npm run build` must succeed; visual verification in dev server  
**Target Platform**: GitHub Pages (static site)  
**Project Type**: Static documentation website  
**Performance Goals**: N/A (build-time only)  
**Constraints**: All content must follow Doks/Hugo conventions; Markdown frontmatter required; start body with H2  
**Scale/Scope**: 3 Markdown files modified; no new files in content tree

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### I. Content Accuracy — PASS

This feature exists specifically to bring website content into alignment with the upstream Dewey v3.0.0 repository. Every change is sourced from PR #42 (spec 013-knowledge-compile), the Dewey README, and `server.go`. The spec explicitly requires all content to be sourced from the upstream repository (FR-014). No features are fabricated or overstated.

Specific accuracy measures:

- Tool count (48) sourced from upstream PR #42
- Source type count (4) sourced from upstream PR #32 and PR #42
- Trust tier definitions sourced from spec 013 Key Entities
- `store_learning` API parameters sourced from PR #42 implementation
- Web crawler note sourced from issue #29 fix

### II. Minimal Footprint — PASS

All changes are Markdown content updates to three existing files. No new files created in the content tree. No new dependencies, no custom CSS, no new layouts, no JavaScript, no new pages. The Doks theme handles all rendering. Content additions use standard Markdown (headings, tables, code blocks, bullet lists) — all natively supported by Doks.

### III. Visitor Clarity — PASS

Updates improve visitor clarity by:

- Correcting tool counts so visitors see accurate capability information (40 → 48)
- Adding source type documentation so visitors know Dewey can index source code
- Documenting compile, lint, and promote so visitors understand the knowledge lifecycle
- Explaining trust tiers so visitors understand how Dewey ensures knowledge quality
- Documenting `store_learning` parameters so agent authors can configure agents correctly
- Resolving cross-page inconsistencies (all pages will agree on tool count and source type count)

The two-click discoverability requirement (SC-002) is met: homepage → Dewey project page → compile/lint/promote descriptions.

## Project Structure

### Documentation (this feature)

```text
specs/020-dewey-v3-docs-sync/
├── plan.md              # This file
├── research.md          # Phase 0 output — upstream v3.0.0 research
├── quickstart.md        # Phase 1 output — verification steps
└── tasks.md             # Phase 2 output (created by /speckit.tasks)
```

No `data-model.md` — this is a content-only spec with no data entities.
No `contracts/` — this is a content-only spec with no external interfaces.

### Source Code (repository root)

```text
content/docs/projects/
└── dewey.md                # Major — tool count, source types, knowledge lifecycle section

content/docs/getting-started/
└── knowledge.md            # Major — CLI commands, trust tiers, store_learning API, web crawler note

content/docs/team/
└── dewey.md                # Medium — tool count, source types, MCP tools table additions
```

**Structure Decision**: Content-only updates to three existing Markdown files. No new files created in the content tree. No new directories. No layout or style changes. The blog post (`dewey-vs-karpathy.md`) is explicitly out of scope per the spec.

## File Change Matrix

| File                                        | Priority | FRs Covered                                                            | Change Summary                                                                                                                                                   |
| ------------------------------------------- | -------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `content/docs/projects/dewey.md`            | P1       | FR-001, FR-004, FR-005                                                 | Tool count 40→48, source types 3→4, add knowledge lifecycle section (compile/lint/promote)                                                                       |
| `content/docs/getting-started/knowledge.md` | P1       | FR-003, FR-004, FR-006, FR-008, FR-009, FR-010, FR-011, FR-012, FR-013 | Tool count update, source types 3→4, add code source config, CLI commands (compile/lint/promote), trust tiers section, store_learning API docs, web crawler note |
| `content/docs/team/dewey.md`                | P1       | FR-002, FR-004, FR-007                                                 | Tool count 40→48, source types 3→4, add compile/lint/promote to MCP tools table, add code source subsection                                                      |

## Cross-Page Consistency Requirements

These values must be identical across all three pages:

| Value             | Current           | Target                  |
| ----------------- | ----------------- | ----------------------- |
| MCP tool count    | 40                | 48                      |
| Category count    | 10                | 12                      |
| Source type count | 3 (three)         | 4 (four)                |
| Source type list  | disk, GitHub, web | disk, GitHub, web, code |

The blog post (`dewey-vs-karpathy.md`) already states "Four source types" and "40 tools" — the tool count there is out of scope per the spec.

## Change Detail by File

### `content/docs/projects/dewey.md`

1. **Line 17**: Update "40 MCP tools across 10 categories" → "48 MCP tools across 12 categories"
2. **Line 38**: Update heading "### 40 MCP Tools Across 10 Categories" → "### 48 MCP Tools Across 12 Categories"
3. **Line 40**: Update body text to match new tool count and list the 12 categories
4. **Line 46**: Update heading "### Three Content Source Types" → "### Four Content Source Types"
5. **Lines 48-51**: Add fourth bullet for code source type
6. **Line 75**: Update "disk, GitHub, and web crawl" → "disk, GitHub, web crawl, and code"
7. **New section**: Add "### Knowledge Lifecycle" section after Key Features describing compile, lint, and promote at evaluator level

### `content/docs/getting-started/knowledge.md`

1. **Line 147**: Update "three pluggable source types" → "four pluggable source types"
2. **New subsection**: Add "### Code" source configuration section after Web Crawl (before "### Updating Sources")
3. **Web source section**: Add note about complex documentation sites (pkg.go.dev) being supported
4. **New section**: Add "## Knowledge Lifecycle" section with CLI commands (`dewey compile`, `dewey lint`, `dewey promote`)
5. **New section**: Add "## Trust Tiers" section explaining authored/validated/draft and search impact
6. **New section**: Add "## Storing and Retrieving Learnings" section with `store_learning` API documentation (parameters, response format, search result metadata)

### `content/docs/team/dewey.md`

1. **Line 35**: Update "40 MCP tools across 10 categories" → "48 MCP tools across 12 categories"
2. **Line 72**: Update "three pluggable source types" → "four pluggable source types"
3. **New subsection**: Add "### Code" source subsection after Web Crawl
4. **MCP tools table**: Add new "Knowledge Management" category with compile, lint, promote tools
5. **MCP tools table**: Add new "Learning" category with store_learning tool

## Risk Assessment

| Risk                                              | Likelihood | Impact | Mitigation                                                                                             |
| ------------------------------------------------- | ---------- | ------ | ------------------------------------------------------------------------------------------------------ |
| Upstream tool count changes before implementation | Low        | Medium | Verify against Dewey README or server.go at implementation time                                        |
| Inline tool count mentions missed                 | Medium     | High   | Search entire site for "40 MCP", "40 tools", "10 categories", "three.\*source" before marking complete |
| Trust tier descriptions inaccurate                | Low        | Medium | Cross-reference with spec 013 Key Entities section                                                     |
| store_learning API parameters wrong               | Low        | High   | Verify against Dewey v3.0.0 server.go or tool registration code                                        |

## Complexity Tracking

No constitution violations. No complexity tracking needed.
