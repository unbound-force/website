# Feature Specification: Blog Section and "Why Contract Coverage" Article

**Feature Branch**: `003-blog-why-gaze`
**Created**: 2026-02-27
**Status**: Draft
**Input**: User description: "Blog Section and Why Contract Coverage Article"

## Clarifications

### Session 2026-02-27

- Q: How many blog posts should the homepage "Latest Articles" section display at maximum? → A: Show the 1 most recent post.

## User Scenarios & Testing _(mandatory)_

### User Story 1 - Reading the "Why Contract Coverage" Article (Priority: P1)

As a developer evaluating test quality tools, I want to read an article explaining why Contract Coverage matters and how Gaze implements it, so that I understand the value proposition before exploring the project further.

**Why this priority**: This is the core deliverable. The article is the reason the blog section exists. Without it, the blog section has no content and violates the zero-waste mandate.

**Independent Test**: Navigate to the blog from the navbar. Click on the "Why Contract Coverage" article. The full article renders with correct formatting, accurate technical content, and working code examples.

**Acceptance Scenarios**:

1. **Given** I click "Blog" in the main navbar, **When** the blog index page loads, **Then** I see a list of articles with the "Why Contract Coverage" post displayed with its title, description, and date.
2. **Given** I click on the "Why Contract Coverage" article, **When** the page loads, **Then** I see the full article with all sections: The Problem with Line Coverage, What Gaze Measures Instead (pipeline, metrics), A Concrete Example (code blocks, comparison tables), How Gaze Differs from Existing Approaches, and CI Gating on What Matters.
3. **Given** the article content, **When** I cross-reference technical claims against the Gaze repository source code, **Then** all facts are accurate: the 5-tier taxonomy (P0-P4) with 38 effect types, the 5 classification signals, the GazeCRAP formula, the four quadrants, and CLI flag names all match the implementation.
4. **Given** the article mentions classification refinement using project documentation, **When** I read the description, **Then** it clearly states this is an optional external workflow via OpenCode agents, not a built-in Gaze feature.
5. **Given** the article includes CLI command examples, **When** I check the commands, **Then** `gaze quality` uses a single package argument (not `./...`), and threshold flags use their actual names (`--min-contract-coverage`, `--max-over-specification`, `--max-crapload`, `--max-gaze-crapload`).
6. **Given** the article, **When** I check code blocks, **Then** all Go code blocks have the `go` language identifier and all bash code blocks have the `bash` identifier, and both render with syntax highlighting.

---

### User Story 2 - Blog Section Infrastructure (Priority: P1)

As a site visitor, I want a blog section accessible from the main navigation so that I can discover and browse articles published by the Unbound Force project.

**Why this priority**: The blog section is the container for the article. Without it, the article has nowhere to live. This is co-priority with the article itself since both are required for delivery.

**Independent Test**: Click "Blog" in the main navbar. The blog index page loads showing available articles.

**Acceptance Scenarios**:

1. **Given** I am on any page, **When** I look at the main navigation bar, **Then** I see a "Blog" link.
2. **Given** I click "Blog" in the navbar, **When** the page loads, **Then** I see a blog index page with a title, description, and a list of published articles.
3. **Given** the blog index page, **When** I look at the article listings, **Then** each article shows its title, date, and description.
4. **Given** the blog section, **When** I check the site RSS feed, **Then** blog posts are included.
5. **Given** the blog section, **When** I search using the site search, **Then** blog articles appear in search results.

---

### User Story 3 - Homepage "Latest Articles" Section (Priority: P2)

As a first-time visitor on the homepage, I want to see featured articles so that I discover the project publishes technical content and can click through to read it.

**Why this priority**: Homepage integration increases article discoverability but is not required for the article to be accessible. The navbar link alone satisfies the two-click navigation rule.

**Independent Test**: Load the homepage. Scroll to find the "Latest Articles" section. Click through to the article.

**Acceptance Scenarios**:

1. **Given** I am on the homepage, **When** I scroll past the Projects section, **Then** I see a "Latest Articles" section showcasing the single most recent blog post.
2. **Given** the "Latest Articles" section, **When** I see an article card, **Then** it displays the article title, publication date, a brief description, and a link to read it.
3. **Given** I click on an article card, **When** the page loads, **Then** I am taken to the full article in the blog section.
4. **Given** the homepage, **When** I view it in dark mode, **Then** the "Latest Articles" section renders correctly with proper contrast and styling.

---

### Edge Cases

- What happens when only one blog post exists? The blog index page MUST display it as a single-item list without looking broken or empty.
- What happens when a blog post has `draft: true`? It MUST be excluded from the blog index, homepage section, and RSS feed in production builds but visible during `npm run dev`.
- What if a future blog post is added without updating the homepage? The homepage "Latest Articles" section MUST dynamically pull from the blog section so new posts appear automatically without template changes.
- What if the blog section is empty (all posts are drafts)? The "Blog" navbar link still works; the index page displays an empty state gracefully.
- What if technical claims in the article become outdated as Gaze evolves? The article includes a publication date to signal when it was written. Content is treated as a point-in-time snapshot that may require updates when Gaze changes significantly.

## Requirements _(mandatory)_

### Functional Requirements

#### Blog Section Infrastructure

- **FR-001**: A blog content section MUST exist at `content/blog/` with a section index page (`content/blog/_index.md`) containing required YAML frontmatter (`title`, `description`, `lead`, `date`, `draft: false`, `weight`, `toc: false`).
- **FR-002**: Blog posts MUST appear in the site's RSS feed and site search results alongside documentation content.
- **FR-003**: The blog section MUST NOT display sidebar navigation. Blog posts are discoverable via the blog index page, site search, and the homepage.
- **FR-004**: A main navigation menu entry for "Blog" MUST be added to the site navigation configuration so it appears in the persistent navbar.
- **FR-005**: Blog post URLs MUST follow the pattern `/blog/<slug>/`.

#### "Why Contract Coverage" Article

- **FR-006**: The article MUST be published as a Markdown file in the blog section with complete YAML frontmatter including `title`, `description`, `lead`, `date`, `draft: false`, `weight`, and `toc: true`.
- **FR-007**: The article body MUST start with `##` (H2). The `title` frontmatter generates H1.
- **FR-008**: All code blocks MUST use fenced syntax with appropriate language identifiers (`go` for Go, `bash` for shell commands).
- **FR-009**: The article MUST NOT claim that Gaze has a built-in AI agent for classification. The reference to AI-assisted classification MUST clearly describe it as an optional external workflow via OpenCode agents that uses project documentation to refine classification confidence.
- **FR-010**: The GazeCRAP formula MUST use consistent notation with the CRAP formula. Both MUST use percentage notation with the `/100` divisor, matching the actual implementation: `GazeCRAP(m) = complexity^2 * (1 - contractCoverage/100)^3 + complexity`.
- **FR-011**: CLI command examples MUST match the actual Gaze CLI interface. Specifically: `gaze quality` MUST use a single package argument (not the multi-package `./...` wildcard), and all threshold flags MUST use their actual names (`--min-contract-coverage`, `--max-over-specification`, `--max-crapload`, `--max-gaze-crapload`).
- **FR-012**: The description of Gaze as a "static analysis tool" MUST include a note that the CRAP scoring feature can optionally invoke test execution to generate line coverage data.
- **FR-013**: All technical claims MUST be verifiable against the Gaze repository source code. This includes: the 5-tier taxonomy (P0-P4) with 38 effect types, the 5 classification signals with weighted 0-100 confidence scoring, the Over-Specification Score, assertion mapping, the four risk quadrants (Q1-Q4), and all CLI flags.
- **FR-014**: The article MUST retain its "why should I care" framing, adapted for a website audience rather than raw technical documentation.

#### Homepage Integration

- **FR-015**: The homepage template MUST include a "Latest Articles" section positioned between the existing Projects section and the Call to Action section.
- **FR-016**: The "Latest Articles" homepage section MUST dynamically source content from the blog section rather than hardcoding specific article references. It MUST display only the 1 most recent published post. Future blog posts replace the displayed post automatically without template changes.
- **FR-017**: The "Latest Articles" section MUST render correctly in both light and dark mode, using theme-compatible styling for colors and spacing.
- **FR-018**: The "Latest Articles" section MUST display at minimum: article title, publication date, and a brief description for each featured post.

#### Build and Navigation Integrity

- **FR-019**: The site MUST build successfully with zero errors after all changes.
- **FR-020**: The blog article MUST be reachable from the homepage within two clicks: either homepage -> Blog (navbar) -> article, or homepage -> "Latest Articles" card -> article.
- **FR-021**: All internal links MUST be valid. No broken links or references to non-existent pages.
- **FR-022**: The existing homepage sections (hero, features, projects, CTA) MUST NOT be visually or functionally altered by the addition of the "Latest Articles" section.

### Key Entities

- **Blog Section**: A top-level content section at `content/blog/` that hosts articles. Unlike the `docs` section, it does not use sidebar navigation. Posts are listed on the index page.
- **Blog Post**: A Markdown file in `content/blog/` with YAML frontmatter. Each post has a title, description, date, and body content. Posts appear on the blog index, in search results, and in the RSS feed.
- **Homepage Article Card**: A UI component in the homepage template that displays a blog post summary (title, date, description) with a link to the full article. Dynamically sourced from the blog section content.

### Assumptions

- The article content is adapted from the existing source file at `temp/why-gaze.md`. The source has been verified against the Gaze repository, and 4 accuracy corrections are required (documented in FR-009 through FR-012).
- The blog section is explicitly requested by the user, satisfying the constitution's condition that a blog should only be added "when explicitly requested and justified."
- No custom template is needed for individual blog posts; the default theme single-page template is expected to handle blog post rendering. A custom template SHOULD only be created if the default rendering is insufficient.
- The "Latest Articles" homepage section requires a modification to the existing custom homepage template.
- Blog posts do not use sidebar navigation (FR-003), relying instead on the blog index page and site search for discoverability.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: The blog section is accessible via a "Blog" link in the main navigation bar on every page of the site.
- **SC-002**: The "Why Contract Coverage" article renders at its blog URL with all sections, code blocks with syntax highlighting, and comparison tables displaying correctly.
- **SC-003**: The site builds successfully with zero errors after all changes are applied.
- **SC-004**: Cross-referencing the published article against the Gaze repository confirms all 4 accuracy corrections have been applied: (1) AI agent claim clarified as external workflow, (2) GazeCRAP formula uses consistent `/100` notation, (3) CLI commands use correct arguments and flag names, (4) "static analysis" description notes the optional test execution for CRAP scoring.
- **SC-005**: The homepage displays a "Latest Articles" section that shows the single most recent blog post with title, date, and description, linking to the full article.
- **SC-006**: The article is discoverable via the site's search functionality.
- **SC-007**: The article appears in the site's RSS feed.
- **SC-008**: All pages (homepage, blog index, article) render correctly in both light and dark mode.
- **SC-009**: No existing pages or navigation items are broken or visually altered by the changes.
- **SC-010**: Every new page has complete YAML frontmatter with all required fields populated.
