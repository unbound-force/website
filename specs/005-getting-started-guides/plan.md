# Implementation Plan: Getting Started Guides

**Branch**: `005-getting-started-guides` | **Date**: 2026-03-22 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/005-getting-started-guides/spec.md`

## Summary

Add 5 role-specific getting started documentation pages to
the Unbound Force website under `content/docs/getting-started/`.
Each page is a standalone Markdown file with Hugo frontmatter,
following existing content conventions. Pages cover developer,
tester, product owner, product manager workflows, and a
cross-role common workflows reference. Content is sourced from
upstream tool documentation and repository artifacts.

## Technical Context

**Language/Version**: Markdown (Hugo content files)
**Primary Dependencies**: Hugo (via @thulite), Doks theme
(`@thulite/doks-core`), existing `content/docs/getting-started/`
section
**Storage**: N/A (static Markdown files)
**Testing**: Manual validation — `npm run build` must succeed,
visual verification in `npm run dev`, dark mode check
**Target Platform**: GitHub Pages (static HTML)
**Project Type**: Static website (Hugo + Doks)
**Performance Goals**: N/A (static site, no runtime)
**Constraints**: Content accuracy (Constitution Principle I),
minimal footprint (Principle II — use Markdown, no custom
HTML/CSS), visitor clarity (Principle III — clear navigation,
logical hierarchy)
**Scale/Scope**: 5 new Markdown pages, ~200-400 lines each

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after
Phase 1 design._

### I. Content Accuracy — PASS

All guide content MUST be derived from actual tool
capabilities sourced from upstream repositories (Gaze
README, Muti-Mind AGENTS.md, Mx F AGENTS.md, Swarm docs,
OpenCode docs). The spec explicitly requires this in
FR-009: "Each guide MUST contain only real, accurate content
derived from actual tool capabilities." No fabricated
features or overstated maturity.

### II. Minimal Footprint — PASS

All pages are standard Markdown files in
`content/docs/getting-started/`. No custom HTML, CSS, SCSS,
JavaScript, or template overrides required. No new
dependencies in `package.json`. The Doks theme's built-in
sidebar navigation automatically discovers and orders the
pages by frontmatter `weight`. This is the simplest possible
implementation for documentation pages.

### III. Visitor Clarity — PASS

Each guide serves a clear audience (developer, tester, PO,
PM) with a specific purpose. The common workflows page ties
the roles together into end-to-end flows. Navigation is
intuitive — all guides are in the existing "Getting Started"
section, discoverable via sidebar. Information hierarchy
flows: homepage → getting started → role-specific guide.
The spec defines FR-008 for sidebar ordering and FR-016 for
cross-reference integrity.

## Project Structure

### Documentation (this feature)

```text
specs/005-getting-started-guides/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── quickstart.md        # Phase 1 output
└── tasks.md             # Phase 2 output (/speckit.tasks)
```

### Source Code (repository root)

```text
content/docs/getting-started/
├── _index.md              # Existing section landing page
├── quick-start.md         # Existing (weight: 20)
├── developer.md           # NEW (weight: 30)
├── tester.md              # NEW (weight: 40)
├── product-owner.md       # NEW (weight: 50)
├── product-manager.md     # NEW (weight: 60)
└── common-workflows.md    # NEW (weight: 70)
```

**Structure Decision**: All new pages are siblings of the
existing `quick-start.md` in the `getting-started/`
directory. This follows the existing pattern (flat directory,
no subdirectories) and uses weight-based ordering to place
role guides after the quick-start overview and common
workflows last (as a reference). No new directories,
partials, shortcodes, or layout overrides needed.

## Complexity Tracking

No constitution violations requiring justification.
