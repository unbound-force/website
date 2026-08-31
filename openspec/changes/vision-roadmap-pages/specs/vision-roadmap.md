## ADDED Requirements

### Requirement: Vision Page

The site MUST publish a Vision page at `/docs/vision/` containing the full content of the org-level `VISION.md` document. The page MUST include Hugo YAML frontmatter with `title`, `description`, `lead`, `date`, `weight: 5`, `toc: true`, and `draft: false`. Body content MUST start with H2 headings (title frontmatter generates H1). The page MUST preserve all substantive content from the source document without additions, removals, or editorial changes.

#### Scenario: Vision page renders correctly

- **GIVEN** a visitor navigates to `https://unboundforce.dev/docs/vision/`
- **WHEN** the page loads
- **THEN** the page displays the full Vision content with a table of contents, all headings render as navigable anchors, and the page renders correctly in both light and dark mode

#### Scenario: Vision page appears in sidebar

- **GIVEN** a visitor is on any docs page
- **WHEN** they view the sidebar navigation
- **THEN** "Vision" appears as the first item (weight 5), above "Getting Started" (weight 10)

### Requirement: Roadmap Page

The site MUST publish a Roadmap page at `/docs/roadmap/` containing the full content of the org-level `ROADMAP.md` document. The page MUST include Hugo YAML frontmatter with `title`, `description`, `lead`, `date`, `weight: 6`, `toc: true`, and `draft: false`. Body content MUST start with H2 headings. The page MUST preserve all substantive content from the source document without additions, removals, or editorial changes.

#### Scenario: Roadmap page renders correctly

- **GIVEN** a visitor navigates to `https://unboundforce.dev/docs/roadmap/`
- **WHEN** the page loads
- **THEN** the page displays the full Roadmap content with a table of contents, all horizon sections are present, and the page renders correctly in both light and dark mode

#### Scenario: Roadmap page appears in sidebar

- **GIVEN** a visitor is on any docs page
- **WHEN** they view the sidebar navigation
- **THEN** "Roadmap" appears as the second item (weight 6), after "Vision" (weight 5) and before "Getting Started" (weight 10)

### Requirement: Main Navigation Entries

The site MUST add "Vision" and "Roadmap" entries to the main navigation bar. "Vision" MUST have weight 2 and link to `/docs/vision/`. "Roadmap" MUST have weight 3 and link to `/docs/roadmap/`. Both MUST appear before "Blog" (weight 5) and "GitHub" (weight 10).

#### Scenario: Main nav displays Vision and Roadmap

- **GIVEN** a visitor is on any page of the site
- **WHEN** they view the main navigation bar
- **THEN** the nav items appear in order: Vision, Roadmap, Blog, GitHub

### Requirement: Docs Sidebar Entries

The site MUST add `[[docs]]` entries for Vision (weight 5, identifier `vision`, url `/docs/vision/`) and Roadmap (weight 6, identifier `roadmap`, url `/docs/roadmap/`) in `menus.en.toml`.

#### Scenario: Sidebar ordering is correct

- **GIVEN** a visitor views the docs sidebar
- **WHEN** they scan the section list
- **THEN** sections appear in order: Vision (5), Roadmap (6), Getting Started (10), Projects (20), The Team (30), Reference (35), Changelog (38), Contributing (40)

### Requirement: Cross-Reference Links

All GitHub issue links (e.g., `https://github.com/unbound-force/unbound-force/issues/509`) and discussion links (e.g., `https://github.com/unbound-force/unbound-force/discussions/399`) in the Roadmap page MUST render as working hyperlinks. The internal cross-reference from Roadmap to Vision MUST use the site-relative path `/docs/vision/` instead of the source repository's `VISION.md`.

#### Scenario: GitHub links are clickable

- **GIVEN** a visitor is on the Roadmap page
- **WHEN** they click a GitHub issue link
- **THEN** they are navigated to the correct GitHub issue page

#### Scenario: Vision cross-reference works

- **GIVEN** a visitor is on the Roadmap page
- **WHEN** they click the reference to the Vision document
- **THEN** they are navigated to `/docs/vision/` on the same site

### Requirement: Hugo Build Success

The site MUST build successfully with `npm run build` (Hugo `--minify --gc`) after these changes are applied. No warnings or errors related to the new pages SHALL be produced.

#### Scenario: Clean build

- **GIVEN** the Vision and Roadmap pages and menu entries have been added
- **WHEN** `npm run build` is executed
- **THEN** the build completes with exit code 0 and no errors

## MODIFIED Requirements

No existing requirements are modified by this change.

## REMOVED Requirements

No existing requirements are removed by this change.
