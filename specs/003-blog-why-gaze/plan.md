# Implementation Plan: Blog Section and "Why Contract Coverage" Article

**Branch**: `003-blog-why-gaze` | **Date**: 2026-02-27 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/003-blog-why-gaze/spec.md`

## Summary

Add a Blog section to the Unbound Force website and publish the "Why Contract Coverage" article as its first post. The blog uses the Doks theme's built-in blog templates (no custom layouts needed for the blog itself). Configuration changes register the section in Hugo's content pipeline, navigation, and RSS. The homepage gains a "Latest Articles" section (showing the 1 most recent post) between the existing Projects and CTA sections. The article content is adapted from `temp/why-gaze.md` with 4 accuracy corrections verified against the Gaze repository source code.

## Technical Context

**Language/Version**: Hugo (via npm/thulite) + Go 1.23 + Node.js >= 20.11.0
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`, `@thulite/images ^3.3.1`, `@thulite/inline-svg ^1.2.0`, `@thulite/seo ^2.4.1`, `@tabler/icons ^3.34.1`
**Storage**: N/A (static site, Markdown files)
**Testing**: Manual — `npm run build` (zero errors) + visual verification via `npm run dev`
**Target Platform**: GitHub Pages (static HTML/CSS/JS)
**Project Type**: Static website (Hugo SSG)
**Performance Goals**: N/A (static site, no runtime)
**Constraints**: Constitution compliance (Content Accuracy, Minimal Footprint, Visitor Clarity)
**Scale/Scope**: 2 new content files, 3 config edits, 1 template edit

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### Pre-Research Gate

| Principle                 | Status | Evidence                                                                                                                                                                                                                                                   |
| ------------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **I. Content Accuracy**   | PASS   | Article content verified against Gaze repository source code (15 claims checked). 4 accuracy corrections documented in FR-009 through FR-012 will be applied during implementation. No placeholder content — the article is fully written.                 |
| **II. Minimal Footprint** | PASS   | Blog section uses Doks built-in templates (`blog/list.html`, `blog/single.html`) — no custom layouts needed. Blog was explicitly requested by the user, satisfying the constitution's condition. Only 3 config edits and 1 template modification required. |
| **III. Visitor Clarity**  | PASS   | Article uses "why should I care" framing (Constitution Principle III). Blog accessible within 2 clicks from homepage (navbar -> blog -> article, or homepage card -> article). Blog entry added to main navbar for discoverability.                        |

**Gate result: PASS — all principles satisfied. Proceeding to Phase 0.**

### Post-Design Gate

| Principle                 | Status | Evidence                                                                                                                                                                                                                                |
| ------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **I. Content Accuracy**   | PASS   | Research confirmed all Gaze claims are verifiable. Correction plan documented in research.md (R4). Frontmatter schema includes publication date for temporal context.                                                                   |
| **II. Minimal Footprint** | PASS   | Research confirmed Doks ships blog templates natively (R1). Zero custom layouts needed for the blog section. Homepage modification is minimal — a single new section block using existing CSS patterns. No new npm dependencies.        |
| **III. Visitor Clarity**  | PASS   | Data model confirms 2-click navigation paths. Blog menu entry at weight 5 positions it visibly in navbar. Homepage "Latest Articles" section provides alternative discovery path. Article title clearly communicates value proposition. |

**Gate result: PASS — all principles satisfied post-design.**

## Project Structure

### Documentation (this feature)

```text
specs/003-blog-why-gaze/
├── spec.md              # Feature specification
├── plan.md              # This file
├── research.md          # Phase 0: technology decisions
├── data-model.md        # Phase 1: entity definitions and file inventory
├── quickstart.md        # Phase 1: implementation quick reference
├── checklists/
│   └── requirements.md  # Specification quality checklist
└── tasks.md             # Phase 2 output (/speckit.tasks — NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
content/
├── _index.md                              # Homepage (existing, unchanged)
└── blog/
    ├── _index.md                          # Blog section index (NEW)
    └── why-contract-coverage.md           # "Why Contract Coverage" article (NEW)

config/_default/
├── hugo.toml                              # Add blog permalink pattern (EDIT)
├── params.toml                            # Add "blog" to mainSections (EDIT)
└── menus/
    └── menus.en.toml                      # Add Blog navbar entry (EDIT)

layouts/
└── home.html                              # Add "Latest Articles" section (EDIT)
```

**Structure Decision**: This feature adds content files and modifies configuration — no new source code directories. The blog section lives under `content/blog/` following Hugo's convention for top-level content sections. The Doks theme's built-in `blog/list.html` and `blog/single.html` templates handle rendering without custom overrides.

## Implementation Phases

### Phase 1: Blog Infrastructure (FR-001 through FR-005)

**Goal**: Register the blog section in Hugo so it builds, appears in navigation, generates correct URLs, and feeds into RSS/search.

**Steps**:

1. Edit `config/_default/params.toml` — change `mainSections = ["docs"]` to `mainSections = ["docs", "blog"]`
2. Edit `config/_default/hugo.toml` — add `blog = "/blog/:slug/"` under `[permalinks]`
3. Edit `config/_default/menus/menus.en.toml` — add `[[main]]` entry: name="Blog", url="/blog/", weight=5
4. Create `content/blog/_index.md` with frontmatter (title, description, date, draft: false, weight: 1, toc: false)
5. Run `npm run build` to verify the blog section builds without errors

**Verification**: Blog index page renders at `/blog/`. "Blog" link appears in the navbar. No build errors.

### Phase 2: Article Content (FR-006 through FR-014)

**Goal**: Publish the "Why Contract Coverage" article with all accuracy corrections applied.

**Steps**:

1. Create `content/blog/why-contract-coverage.md` by adapting `temp/why-gaze.md`:
   - Add YAML frontmatter (title, description, lead, date, draft: false, weight: 100, toc: true, categories, tags, contributors)
   - Remove the H1 title from body (frontmatter generates it)
   - Ensure all code blocks use fenced syntax with language identifiers (`go`, `bash`)
2. Apply accuracy correction FR-009: Reword AI agent claim to describe external OpenCode workflow
3. Apply accuracy correction FR-010: Add `/100` divisor to GazeCRAP formula for consistent notation
4. Apply accuracy correction FR-011: Fix CLI command examples — single package for `gaze quality`, correct flag names
5. Apply accuracy correction FR-012: Add caveat about optional test execution in CRAP scoring
6. Run `npm run build` to verify article renders without errors

**Verification**: Article renders at `/blog/why-contract-coverage/` with all sections, code blocks with syntax highlighting, and tables. All 4 corrections applied and verifiable against Gaze source.

### Phase 3: Homepage Integration (FR-015 through FR-018, FR-022)

**Goal**: Add a "Latest Articles" section to the homepage that dynamically shows the most recent blog post.

**Steps**:

1. Edit `layouts/home.html` — add a new section block between the Projects section (ending line 111) and the CTA section (starting line 113)
2. Use Hugo template logic: `{{ with site.GetPage "/blog" }}` to access the blog section, then `{{ range first 1 .Pages.ByDate.Reverse }}` to get the most recent post
3. Render an article card with title (`.Title`), date (`.Date`), description (`.Params.description`), and link (`.Permalink`)
4. Style the section consistently with the existing Projects section (same container, spacing, responsive grid)
5. Use Bootstrap CSS variables (`var(--bs-*)`) for dark mode compatibility

**Verification**: Homepage shows "Latest Articles" section between Projects and CTA. Card displays title, date, description. Clicking links to the full article. Dark mode renders correctly. Existing sections unchanged.

### Phase 4: Final Verification (FR-019 through FR-022)

**Goal**: Confirm all success criteria are met.

**Steps**:

1. `npm run build` — zero errors
2. `npm run dev` — visual verification:
   - Blog navbar link present on all pages
   - Blog index lists the article with title, date, description
   - Article renders fully with code blocks, tables, syntax highlighting
   - Homepage "Latest Articles" shows the article
   - Dark mode correct on all new/modified pages
   - Existing pages (homepage hero, features, projects, CTA) unchanged
3. Verify article appears in RSS feed output
4. Verify article appears in search index

## Risk Assessment

| Risk                                                                | Impact | Likelihood | Mitigation                                                                                                         |
| ------------------------------------------------------------------- | ------ | ---------- | ------------------------------------------------------------------------------------------------------------------ |
| Doks blog templates render unexpectedly                             | Medium | Low        | Templates are well-documented; fallback to root-level `list.html`/`single.html` if needed                          |
| Homepage template change breaks existing sections                   | High   | Low        | Add new section as an independent block between existing sections; FR-022 requires regression check                |
| Blog search integration doesn't work                                | Low    | Low        | Adding `"blog"` to `mainSections` should handle this; FlexSearch indexes all mainSections                          |
| Article content has additional inaccuracies beyond the 4 identified | Medium | Low        | 15 claims were systematically verified; remaining content is descriptive prose without verifiable technical claims |
