## Why

GitHub issue [#270](https://github.com/unbound-force/website/issues/270) requests publishing the org-level `VISION.md` and `ROADMAP.md` documents as pages on unboundforce.dev. These documents define what Unbound Force is, where it's going, and why — but they're currently buried in the `unbound-force/unbound-force` repository where website visitors can't find them. Making them prominent website pages gives visitors immediate access to the project's direction and maturity.

## What Changes

- **Two new documentation sections** added to the site: Vision (`content/docs/vision/_index.md`) and Roadmap (`content/docs/roadmap/_index.md`).
- **Main navigation bar** updated to include Vision and Roadmap links (before Blog).
- **Docs sidebar** updated with Vision (weight 5) and Roadmap (weight 6) at the top, before Getting Started (weight 10).
- Content adapted from source Markdown to Hugo frontmatter format. Substance preserved exactly; only formatting adjusted for the Doks theme.
- Internal cross-reference (`[VISION.md](VISION.md)` in ROADMAP) rewritten as site-relative link (`[Vision](/docs/vision/)`).
- All external GitHub issue/discussion links preserved as full URLs.

## Capabilities

### New Capabilities
- `vision-page`: Publishes the Unbound Force vision statement as a docs page at `/docs/vision/`
- `roadmap-page`: Publishes the project roadmap as a docs page at `/docs/roadmap/`
- `nav-visibility`: Both pages accessible from main nav bar and docs sidebar

### Modified Capabilities
- `site-navigation`: Main nav gains two new entries (Vision weight 2, Roadmap weight 3); docs sidebar gains two new sections (weights 5 and 6)

### Removed Capabilities
- None

## Impact

- **Files created**: `content/docs/vision/_index.md`, `content/docs/roadmap/_index.md`
- **Files modified**: `config/_default/menus/menus.en.toml`
- **No code changes**: This is a docs-only change — Markdown content and TOML config only
- **No build system impact**: Standard Hugo content pages using Doks theme defaults
- **Navigation shift**: Existing sidebar sections (Getting Started, Projects, etc.) remain at their current weights but shift visually downward as Vision and Roadmap appear above them

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

This change adds static documentation pages to the website. It does not introduce runtime coupling or modify artifact-based communication between heroes.

### II. Composability First

**Assessment**: PASS

The new pages are standard Hugo Markdown files with no custom layouts or dependencies. They use the Doks theme's built-in rendering. Either page can be added or removed independently without affecting the other or any existing pages.

### III. Observable Quality

**Assessment**: N/A

This change produces static HTML documentation. It does not generate machine-parseable output or require provenance metadata — it publishes existing organizational documents.

### IV. Testability

**Assessment**: PASS

The change is validated by `npm run build` (Hugo build succeeds), visual inspection (pages render correctly in light and dark mode), and link verification (all cross-references resolve). No external services required.
