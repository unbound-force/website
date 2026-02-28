# Implementation Plan: Documentation Content Pages

**Branch**: `002-documentation-content` | **Date**: 2026-02-27 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/002-documentation-content/spec.md`

## Summary

Create all documentation content sections for the Unbound Force website: Getting Started (2 pages), Projects (2 pages), Team (6 pages), and Contributing (1 page), plus the docs-level `_index.md`. Content is adapted from upstream source repositories (`unbound-force/unbound-force.md` for personas, `unbound-force/gaze` README for the Gaze project page). All pages use standard Markdown with YAML frontmatter — no custom layouts, shortcodes, or SCSS required. The existing navigation menu (`menus.en.toml`) already defines the four documentation sections; this feature populates them with real content.

## Technical Context

**Language/Version**: Hugo (via npm/thulite) + Go 1.23 (module resolution) + Node.js >= 20.11.0
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`, `@thulite/images ^3.3.1`, `@thulite/inline-svg ^1.2.0`, `@thulite/seo ^2.4.1`, `@tabler/icons ^3.34.1`
**Storage**: N/A (static site, Markdown files)
**Testing**: Manual — `npm run build` must succeed; visual verification via `npm run dev`
**Target Platform**: GitHub Pages (static hosting)
**Project Type**: Static documentation website (Hugo SSG)
**Performance Goals**: N/A (static site, no runtime performance concerns)
**Constraints**: Zero placeholder content; all facts must be verifiable against source repos; Doks built-in features preferred over custom implementations
**Scale/Scope**: 12 new Markdown pages across 4 documentation sections

### Key Technical Decisions

- **Markdown only**: All 12 pages are standard `.md` files with YAML frontmatter. No custom Hugo templates, shortcodes, or partials needed.
- **Image rendering**: Team persona trading card images use standard Markdown `![alt](/images/team/filename.png)` syntax. Images already exist in `static/images/team/`.
- **Navigation**: The `menus.en.toml` already defines the four `[[docs]]` sections with correct weights. Hugo auto-discovers pages within sections via `_index.md` files.
- **Blog cross-link**: The Gaze project page links to `/blog/why-contract-coverage/` which already exists as `content/blog/why-contract-coverage.md`.
- **Gaze presentation**: Per clarification, Gaze is presented exclusively as an OpenCode-integrated tool via the `/gaze` command. CLI commands (`gaze analyze`, etc.) are NOT documented on the website. The `/gaze` OpenCode command interface needs research (Phase 0).
- **"Get Started" button**: The navbar "Get Started" button is currently disabled in `params.toml` with the comment "until docs content exists (spec 002)". It should be re-enabled once getting-started content is published.

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### I. Content Accuracy — PASS

All content is derived from verified source material:

- Persona descriptions adapted from `unbound-force/unbound-force.md` (fetched and verified)
- Gaze project content adapted from `unbound-force/gaze` README (fetched and verified)
- No features are fabricated; known limitations are documented per FR-005
- No placeholder or "Coming Soon" content — all 12 pages will contain substantive content

### II. Minimal Footprint — PASS

- All pages are standard Markdown — no custom HTML templates, shortcodes, or SCSS
- Images use standard Markdown syntax — no custom image components
- No new dependencies added to `package.json`
- Existing `menus.en.toml` navigation entries are reused, not duplicated
- Doks built-in sidebar navigation handles page discovery automatically via `_index.md` section indexes

### III. Visitor Clarity — PASS

- Information hierarchy follows the spec: Getting Started → Projects → Team → Contributing
- Every page has a clear purpose articulated in the spec's user stories
- All pages are reachable within two clicks from the homepage via sidebar navigation
- Content is adapted for a website audience — "why should I care" framing, not raw README text
- The Getting Started section provides the "Get Started" entry point for the homepage CTA

### Gate Result: **ALL PASS — proceed to Phase 0**

## Project Structure

### Documentation (this feature)

```text
specs/002-documentation-content/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
content/
├── _index.md                              # Homepage (exists)
├── blog/                                  # Blog section (exists)
│   ├── _index.md
│   └── why-contract-coverage.md
└── docs/                                  # Documentation sections (NEW — this feature)
    ├── _index.md                          # Docs landing page
    ├── getting-started/
    │   ├── _index.md                      # "What is Unbound Force?"
    │   └── quick-start.md                 # Quick Start guide
    ├── projects/
    │   ├── _index.md                      # Projects overview
    │   └── gaze.md                        # Gaze project page
    ├── team/
    │   ├── _index.md                      # "The Heroes" overview
    │   ├── muti-mind.md                   # Product Owner persona
    │   ├── cobalt-crush.md                # Developer persona
    │   ├── gaze-tester.md                 # Tester persona
    │   ├── the-divisor.md                 # PR Reviewer persona
    │   └── mx-f.md                        # Manager persona
    └── contributing/
        └── _index.md                      # Contributing guide

config/_default/
├── params.toml                            # Re-enable "Get Started" button
└── menus/menus.en.toml                    # Already configured (no changes needed)

static/images/team/                        # Trading card images (exist)
├── the-unbound-force.jpeg                 # Team composite
├── multi-mind.png                         # Muti-Mind trading card
├── cobalt-crush.png                       # Cobalt-Crush trading card
├── gaze.png                               # Gaze trading card
├── the-divisor.png                        # The Divisor trading card
└── mx-f.png                               # Mx F trading card
```

**Structure Decision**: Hugo content-as-structure model. Each section has a `_index.md` for the section landing page and individual `.md` files for sub-pages. No `src/`, `tests/`, or other code directories — this is a content-only feature. The docs-level `_index.md` is required so Hugo treats `content/docs/` as a section root.

## Constitution Check — Post-Design Re-evaluation

_Re-check after Phase 1 design completion._

### I. Content Accuracy — PASS

- All content sources verified via direct fetch from upstream repos:
  - `unbound-force/unbound-force.md` — all 5 persona descriptions confirmed (Muti-Mind, Cobalt-Crush, Gaze, The Divisor, Mx F)
  - `unbound-force/gaze` README — all metrics (Contract Coverage, Over-Specification, GazeCRAP), architecture, limitations confirmed
  - `/gaze` OpenCode command interface — researched via spec 005 and scaffold source; 3 modes (full/crap/quality), `gaze init` scaffolding confirmed
- Known Gaze limitations documented honestly: direct function body only, P3-P4 not detected, ~74% assertion mapping accuracy, no CGo/unsafe, single package loading
- No fabricated features in any design artifact
- Blog cross-link `/blog/why-contract-coverage/` confirmed to exist

### II. Minimal Footprint — PASS

- Design adds zero custom templates, shortcodes, SCSS, or JavaScript
- All 12 pages are standard Markdown with YAML frontmatter
- Images use standard Markdown syntax (Doks render hook provides optimization automatically)
- Only config change: setting `navBarButton = true` in `params.toml` (currently `false`)
- No new `package.json` dependencies
- No `contracts/` directory needed — static content has no external interfaces

### III. Visitor Clarity — PASS

- 12-page content inventory with clear purpose for every page (documented in data-model.md)
- Weight scheme ensures deterministic sidebar ordering (10/20/30... within sections)
- Getting Started → Quick Start provides the homepage CTA conversion path
- Gaze project page links to blog for deeper technical content — layered information depth
- Team overview → persona pages provides progressive disclosure of agent details
- "Get Started" navbar button re-enabled to complete the homepage → docs navigation path

### Post-Design Gate Result: **ALL PASS — no violations, no complexity tracking needed**

## Complexity Tracking

> No constitution violations detected — this section is intentionally empty.

| Violation | Why Needed | Simpler Alternative Rejected Because |
| --------- | ---------- | ------------------------------------ |
| _(none)_  | —          | —                                    |
