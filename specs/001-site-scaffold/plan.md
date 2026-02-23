# Implementation Plan: Site Scaffold and Deployment

**Branch**: `001-site-scaffold` | **Date**: 2026-02-23 | **Spec**: `specs/001-site-scaffold/spec.md`
**Input**: Feature specification from `specs/001-site-scaffold/spec.md`

## Summary

Build the complete Hugo + Doks/Thulite site infrastructure from scratch: package.json, go.mod, Hugo configuration, custom homepage template, brand SCSS, CI/CD workflow, and CNAME for unboundforce.dev. The repository currently contains only planning artifacts (specs, constitution, AGENTS.md) and no implementation files. The technical approach mirrors the complytime-website reference implementation with Unbound Force-specific content, colors, and structure.

## Technical Context

**Language/Version**: Hugo (via npm/thulite) + Go 1.23 (module resolution) + Node.js >= 20.11.0
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`, `@thulite/images ^3.3.1`, `@thulite/inline-svg ^1.2.0`, `@thulite/seo ^2.4.1`, `@tabler/icons ^3.34.1`
**Dev Dependencies**: `prettier ^3.6.2`, `vite ^7.0.6`
**Storage**: N/A (static site, no database)
**Testing**: Manual verification (`npm run build` + visual inspection). No automated test suite. Lighthouse >= 90 for performance. WCAG 2.1 AA compliance.
**Target Platform**: GitHub Pages (static hosting), all modern browsers
**Project Type**: Static website (Hugo SSG)
**Performance Goals**: Lighthouse performance score >= 90 on mobile and desktop
**Constraints**: WCAG 2.1 AA accessibility (4.5:1 contrast ratio), no external fonts, SCSS limited to two files, Doks built-in features preferred over custom code
**Scale/Scope**: Single-page homepage + documentation section structure (Getting Started, Projects, Team, Contributing). Content pages are out of scope for this spec (covered by spec 002).

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

### Pre-Phase 0 Gate

| Principle | Status | Assessment |
|-----------|--------|------------|
| I. Content Accuracy | PASS | The Gaze project card description will be derived from the actual Gaze repository README. Research confirms Gaze does P0-P2 side effect detection and CRAP scoring for Go. GazeCRAP (contract-aware coverage) is NOT yet implemented and MUST NOT be claimed. No placeholder or "Coming Soon" content will be created. |
| II. Minimal Footprint | PASS | The implementation mirrors the complytime-website reference (proven minimal stack). Custom code is limited to `layouts/home.html` (required since Doks has no homepage out of the box), two SCSS files (brand colors + homepage card styles), and standard Hugo config. No additional npm dependencies beyond what complytime-website uses. No blog, analytics, or ancillary features. |
| III. Visitor Clarity | PASS | The homepage follows a clear 4-section structure (hero, features, projects, CTA) designed for immediate comprehension. Navigation menus provide two-click access to all content sections. Information hierarchy flows homepage -> projects -> detailed docs as required. |

**Gate result: ALL PASS. Proceeding to Phase 0.**

## Project Structure

### Documentation (this feature)

```text
specs/001-site-scaffold/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
└── tasks.md             # Phase 2 output (/speckit.tasks - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
website/
├── .github/
│   └── workflows/
│       └── deploy-gh-pages.yml       # CI/CD: build + deploy on push to main
├── assets/
│   └── scss/common/
│       ├── _variables-custom.scss    # Brand colors (Bootstrap variable overrides)
│       └── _custom.scss              # Homepage card/section styles
├── config/
│   └── _default/
│       ├── hugo.toml                 # Core Hugo settings (title, baseURL, outputs)
│       ├── module.toml               # Hugo module mounts (node_modules -> Hugo virtual FS)
│       ├── params.toml               # Doks theme parameters (colors, search, repo link)
│       └── menus/
│           └── menus.en.toml         # Navigation menu definitions
├── content/
│   └── _index.md                     # Homepage frontmatter only
├── layouts/
│   └── home.html                     # Custom homepage template (hero, features, projects, CTA)
├── static/
│   ├── CNAME                         # Custom domain: unboundforce.dev
│   ├── favicon.png                   # Site favicon
│   └── favicon-512x512.png           # Large favicon for PWA/social
├── .gitignore                        # Excludes public/, node_modules/, etc.
├── go.mod                            # Hugo module resolution (Go 1.23)
├── go.sum                            # Go module checksums
└── package.json                      # Node dependencies and npm scripts
```

**Structure Decision**: Static site with Hugo SSG. No src/, tests/, or backend directories needed. All "code" is Hugo templates (home.html), SCSS customization, and configuration (TOML). The `module.toml` file is critical -- it wires node_modules from Thulite packages into Hugo's virtual filesystem and explicitly excludes Doks' default `home.html` to allow our custom homepage.

## Complexity Tracking

> No constitution violations detected. Table intentionally empty.

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| Custom `layouts/home.html` | Doks does not provide a homepage layout; only docs pages | No simpler alternative exists -- a custom homepage template is the standard Doks pattern |
| Custom `_custom.scss` | Homepage cards/sections need styling not provided by Doks theme | Using inline styles would be harder to maintain and violate Doks conventions |
| `module.toml` with explicit mounts | Required to wire Thulite npm packages into Hugo and exclude Doks' default home.html | This is the documented Thulite/Doks architecture, not a custom workaround |

## Constitution Check — Post-Design Re-Evaluation

*Re-check after Phase 1 design completion.*

| Principle | Status | Post-Design Assessment |
|-----------|--------|------------------------|
| I. Content Accuracy | PASS | Research verified Gaze's actual capabilities (P0-P2 side effects, CRAP scores). Plan explicitly excludes GazeCRAP and unimplemented features. Homepage content derived from verified repository facts. No placeholder content created. |
| II. Minimal Footprint | PASS | Custom code limited to: 1 template (home.html — required, Doks has no homepage), 2 SCSS files (brand colors + card styles), standard Hugo config + module.toml (standard Thulite architecture). No additional npm dependencies. No blog, analytics, or ancillary features. |
| III. Visitor Clarity | PASS | Homepage 4-section structure (hero → features → projects → CTA) provides immediate comprehension. Navigation menus enable two-click access to all sections. Information hierarchy flows homepage → projects → detailed docs. |

**Post-design gate result: ALL PASS. No new violations introduced by design decisions.**
