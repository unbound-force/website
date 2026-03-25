# Implementation Plan: Dewey Documentation

**Branch**: `007-dewey-docs` | **Date**: 2026-03-25 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/007-dewey-docs/spec.md`

## Summary

Add Dewey documentation to the Unbound Force website:
a getting-started guide for installation and
configuration, a tool page describing Dewey's
architecture and capabilities, updates to all
role-specific guides and the common workflows page to
reference Dewey's role, and navigation updates to make
the new pages discoverable.

Content is derived from the Dewey design paper
(`../dewey-design-paper.md`) and architectural spec
(Spec 014 in the meta repo), adapted for a website
audience per Constitution Principle III (Visitor
Clarity).

## Technical Context

**Language/Version**: Hugo (via npm/thulite) + Go 1.23
(module resolution) + Node.js >= 20.11.0
**Primary Dependencies**: `thulite ^2.6.3`,
`@thulite/doks-core ^1.8.3`
**Storage**: N/A (static site, Markdown files)
**Testing**: Manual: `npm run build` must succeed,
`npm run dev` for visual verification
**Target Platform**: GitHub Pages (static site)
**Project Type**: Hugo static site (Doks theme)
**Performance Goals**: N/A (static content)
**Constraints**: Markdown preferred over custom HTML
(Constitution Principle II). Content must be derived
from upstream repos (Constitution Principle I). No
placeholder content.
**Scale/Scope**: 2 new pages + 5 updated pages +
navigation config update. ~500 lines of new Markdown
content total.

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check
after Phase 1 design._

### I. Content Accuracy -- PASS

All Dewey content will be derived from the design paper
and Spec 014 in the meta repo. Dewey's core
implementation is complete (86/86 tasks in the dewey
repo). The capabilities described (persistence, semantic
search, content sources, 3-tier degradation) are
implemented and tested. No features will be overstated.

The getting-started guide will include both Homebrew
and `go install` installation options since the Homebrew
formula may not yet be published when the docs go live.
This is transparent, not misleading.

### II. Minimal Footprint -- PASS

All new content is Markdown. No custom HTML, SCSS,
JavaScript, or template overrides. Navigation is
configured via the existing `menus.en.toml` file. The
Doks theme provides sidebar navigation, dark mode,
and responsive layout -- no custom code needed.

### III. Visitor Clarity -- PASS

The getting-started guide is self-contained: a developer
can read it without prior knowledge of the hero pages.
The tool page provides deeper reference. The workflow
pages are updated incrementally (adding Dewey context
to existing sections, not restructuring). Navigation
is updated so new pages are discoverable within 2
clicks from the homepage.

## Project Structure

### Documentation (this feature)

```text
specs/007-dewey-docs/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── quickstart.md        # Phase 1 output
└── checklists/
    └── requirements.md  # Already created
```

### Content (repository root)

```text
content/docs/
├── getting-started/
│   ├── knowledge.md          # NEW: Dewey getting-started guide
│   ├── common-workflows.md   # UPDATED: Dewey context
│   ├── developer.md          # UPDATED: Cobalt-Crush + Dewey
│   ├── product-manager.md    # UPDATED: Mx F + Dewey
│   ├── product-owner.md      # UPDATED: Muti-Mind + Dewey
│   └── tester.md             # UPDATED: Gaze + Dewey
└── team/
    └── dewey.md              # NEW: Dewey tool page

config/_default/
└── menus/menus.en.toml       # UPDATED: navigation entries
```

**Structure Decision**: All content is standard Hugo
Markdown pages in the existing directory structure. No
new directories, templates, or layouts. The `knowledge.md`
name was chosen to be descriptive without implying a
specific tool name in the URL path.

## Complexity Tracking

No constitution violations. No complexity justifications
needed.
