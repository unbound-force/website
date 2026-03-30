# Implementation Plan: Project Documentation Update

**Branch**: `012-project-docs-update` | **Date**: 2026-03-30 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `specs/012-project-docs-update/spec.md`

## Summary

Update all project documentation on the Unbound Force website to match current tool versions — Gaze v1.4.7, Dewey v1.4.2, and swarm workflow features (/unleash, /finale, review council CI gate, Divisor refinements, opencode.json management, severity pack). This is a documentation-only change affecting 6 Markdown content pages. No code, no data model, no contracts — pure content accuracy work driven by three audit documents.

## Technical Context

**Language/Version**: Hugo (Go-based SSG) via `@thulite` npm packages; Node.js >= 20.11.0; Go >= 1.23
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`
**Storage**: N/A (static Markdown files)
**Testing**: `npm run build` must succeed without errors; visual verification via `npm run dev`
**Target Platform**: GitHub Pages (static site)
**Project Type**: Documentation website (Hugo + Doks theme)
**Performance Goals**: N/A (static site generation)
**Constraints**: All content must be derived from upstream repository sources (Constitution Principle I: Content Accuracy)
**Scale/Scope**: 6 content pages updated, 0 new pages, 0 code changes

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### I. Content Accuracy — PASS (directly improved)

This spec exists specifically to fix content accuracy violations. The gap analysis documents identify 3 critical factual errors (wrong file count, removed command reference, stale accuracy number), 5 stale references, 4 missing major features, and multiple missing CLI commands. Every change is sourced from authoritative upstream repositories and audit documents. After implementation, zero factually incorrect statements should remain.

### II. Minimal Footprint — PASS

No new pages are created. No custom HTML, CSS, or JavaScript is added. All changes are Markdown content updates to existing pages. The Doks theme's built-in features (tables, code blocks, headings) are sufficient for all new content. No dependencies are added.

### III. Visitor Clarity — PASS (directly improved)

This spec improves visitor clarity by:

- Fixing instructions that don't match actual tool behavior (gaze init file count, removed /classify-docs command)
- Adding documentation for discoverable features (/unleash, /finale, dewey doctor, /gaze fix)
- Adding troubleshooting guidance for common issues (MCP timeout, Ollama not running)
- Documenting the complete CI gate pipeline so developers understand what happens before code review

## Project Structure

### Documentation (this feature)

```text
specs/012-project-docs-update/
├── plan.md              # This file
├── research.md          # Phase 0: source material from all 3 areas
├── quickstart.md        # Post-implementation verification steps
├── checklists/
│   └── requirements.md  # Specification quality checklist
└── tasks.md             # Phase 2 output (created by /speckit.tasks)
```

### Source Code (repository root)

```text
content/docs/
├── projects/
│   ├── gaze.md              # Updated: US1, US2, US3
│   └── dewey.md             # Minor: version context (if needed)
├── getting-started/
│   ├── tester.md            # Updated: US2, US3
│   ├── knowledge.md         # Updated: US4, US5
│   ├── common-workflows.md  # Updated: US6, US7, US8
│   └── developer.md         # Updated: US7, US8
└── team/
    └── the-divisor.md       # Updated: US7
```

**Structure Decision**: All updates go into existing pages. No new pages are created. This follows the spec's assumptions that /unleash and /finale are added to common-workflows.md, Dewey troubleshooting is added to knowledge.md, and severity pack details are added to developer.md. This keeps related information co-located and avoids page proliferation.

## Phase 0: Research

See [research.md](research.md) for comprehensive source material covering:

1. **Gaze v1.4.7** — 3 critical errors, 5 stale references, 4 missing major features, 5 missing significant features. Sourced from `gaze-website-docs-audit.md`.
2. **Dewey v1.4.2** — `.gitignore` support (Spec 006), missing CLI commands (doctor, reindex), global flags, troubleshooting. Sourced from `dewey-gaps-2026-03-30.md`.
3. **Swarm workflow** — `/unleash` (8-step pipeline), `/finale` (9-step workflow), review council CI gate (Phase 1a/1b), Divisor refinements (exclusive ownership, severity pack, prior learnings), `uf init` opencode.json management. Sourced from upstream command files and spec 019.

## Phase 1: Design

### Content Architecture

This is a documentation-only change. The "design" is the content organization strategy:

**Gaze pages** (gaze.md, tester.md):

- Fix factual errors inline (file count, accuracy number, removed command)
- Add missing features as new subsections or table rows
- Update existing sections with corrected values

**Dewey page** (knowledge.md):

- Update Local Disk section with `.gitignore` behavior and new YAML fields
- Add `dewey doctor` and `dewey reindex` to the Initialize Your Repository section
- Add global CLI flags documentation
- Add a new Troubleshooting section at the end (before Next Steps)

**Workflow pages** (common-workflows.md, developer.md):

- Add `/unleash` and `/finale` as new sections in common-workflows.md (after Bug Fix, before Code Review)
- Update Code Review section with Phase 1a/1b details
- Update convention pack count in developer.md
- Update `uf init` and `uf setup` documentation

**Team page** (the-divisor.md):

- Add exclusive ownership model
- Add severity standard reference
- Add prior learnings integration

### Data Model

**Skipped** — this is a documentation-only change. No data model, no database, no schemas.

### Contracts

**Skipped** — no API contracts, no inter-service communication, no versioned schemas.

### Quickstart

See [quickstart.md](quickstart.md) for verification steps.

## Coverage Strategy

This is a static site documentation change. "Coverage" means:

1. **Build verification**: `npm run build` must succeed without errors after all changes
2. **Cross-page consistency**: Key claims (pack count, accuracy numbers, file counts) must be consistent across all pages that reference them
3. **Link integrity**: No broken internal links introduced by content changes
4. **Visual verification**: `npm run dev` and spot-check all modified pages render correctly

No unit tests, integration tests, or automated test suites apply to this change.

## Constitution Re-Check (Post-Design)

_Re-evaluated after Phase 0 research and Phase 1 design._

### I. Content Accuracy — PASS (confirmed, strengthened)

Research phase confirmed all source material is available and authoritative. Every claim to be added or updated has a traceable source:

- Gaze claims → `gaze-website-docs-audit.md` cross-referenced against Gaze v1.4.7 codebase
- Dewey claims → `dewey-gaps-2026-03-30.md` cross-referenced against Dewey v1.4.2 + main branch
- Swarm claims → upstream command files (`unleash.md`, `finale.md`), Spec 019, `scaffold.go`

No claims are fabricated or extrapolated beyond source material.

### II. Minimal Footprint — PASS (confirmed)

Design confirms zero new pages, zero custom elements, zero dependencies. All content uses standard Markdown with Doks-native formatting (tables, code blocks, headings). The troubleshooting section in knowledge.md uses a simple table format — no custom shortcodes or components needed.

### III. Visitor Clarity — PASS (confirmed, strengthened)

Design confirms information hierarchy is maintained:

- `/unleash` and `/finale` are placed in common-workflows.md (where workflow documentation lives)
- Troubleshooting is placed in knowledge.md (co-located with setup instructions)
- Severity pack is placed in developer.md convention packs section (where pack documentation lives)
- No deep navigation structures introduced — all new content is within existing pages at appropriate heading levels

## Complexity Tracking

No constitution violations to justify. All changes align with the three principles.
