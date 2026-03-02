# Feature Specification: "Gaze in Practice" Blog Post and Docs Sample Output

**Feature Branch**: `004-gaze-in-practice`
**Created**: 2026-03-02
**Status**: Draft
**Input**: User description: "Add documentation/blog content showing real Gaze output from running it on the gcal-organizer project. Both a new blog post walking through the output and a sample output section added to the existing Gaze project docs page."

## Clarifications

### Session 2026-03-02

- Q: Blog post title? → A: "Gaze in Practice: A Real-World Quality Report Walkthrough"
- Q: Should emoji characters from the Gaze output (🔍, 📊, 🧪, 🏥, 🔴, 🟢, 🟡) be preserved in the blog post? → A: Yes, preserve them exactly as Gaze produces them — authenticity matters.
- Q: Should the Quality Summary and Classification Summary sections (which contain "insufficient data" warnings) be included? → A: Yes, include all sections with honest interpretation — demonstrates the tool's transparent reporting.
- Q: Where should the sample output section go on the docs page? → A: After "Three Modes" and before "Architecture" — natural flow from explaining modes to showing what output looks like.

## User Scenarios & Testing _(mandatory)_

### User Story 1 - Reading the "Gaze in Practice" Blog Post (Priority: P1)

As a developer considering Gaze for my Go project, I want to see what a real Gaze report looks like when run against an actual codebase, so that I understand what the tool produces and how to interpret its output before investing time in setting it up.

**Why this priority**: The existing documentation explains concepts (Contract Coverage, GazeCRAP, quadrants) but never shows real output. A walkthrough of actual output is the most effective way to bridge the gap between "what does this tool do?" and "what will I actually see when I use it?" This is the primary deliverable.

**Independent Test**: Navigate to the blog index from the navbar. Click on the "Gaze in Practice" article. The full article renders with the real report output, section-by-section interpretation, and correct formatting.

**Acceptance Scenarios**:

1. **Given** I click "Blog" in the main navbar, **When** the blog index page loads, **Then** I see the "Gaze in Practice" post listed alongside the existing "Why Contract Coverage" post, with its title, description, and date.
2. **Given** I click on the "Gaze in Practice" article, **When** the page loads, **Then** I see all report sections from the real Gaze output: CRAP Summary, Top 5 Worst CRAP Scores, GazeCRAP Quadrant Distribution, Quality Summary, Classification Summary, and Overall Health Assessment.
3. **Given** the article includes output tables, **When** I read them, **Then** the data matches the actual Gaze output from the gcal-organizer project (137 functions analyzed, average complexity 4.9, CRAPload 40, etc.) — no fabricated or modified numbers.
4. **Given** each section of output, **When** I read the surrounding narrative, **Then** there is interpretive text explaining what the numbers mean, why they matter, and what action I would take based on them.
5. **Given** the article, **When** I check the report metadata, **Then** it identifies the project (gcal-organizer), branch (008-decision-extraction), Gaze version (v1.2.3), Go version (1.24.12), and date (2026-03-02).
6. **Given** the article references concepts like GazeCRAP, Contract Coverage, or quadrants, **When** I want to learn more, **Then** there are links back to the existing "Why Contract Coverage" blog post and/or the Gaze project docs page.

---

### User Story 2 - Seeing Sample Output on the Gaze Project Docs Page (Priority: P2)

As a developer scanning the Gaze documentation page, I want to see a condensed example of what the tool produces, so that I can quickly understand the output format without reading a full blog post.

**Why this priority**: The docs page is the primary reference for Gaze. Adding a visual example of output makes it self-contained for quick evaluation. However, the blog post provides the deeper walkthrough, so this is secondary.

**Independent Test**: Navigate to Docs > Projects > Gaze. Scroll to a new "Sample Output" or "What You'll See" section. A condensed excerpt of real output is displayed.

**Acceptance Scenarios**:

1. **Given** I am on the Gaze project docs page, **When** I scroll through the page, **Then** I see a new section showing a condensed sample of real Gaze output.
2. **Given** the sample output section, **When** I read it, **Then** it includes at minimum: the Health Assessment scorecard (dimensions, grades, details) and the Top 5 Worst CRAP Scores table — enough to show the flavor of the tool without overwhelming the docs page.
3. **Given** the sample output section, **When** I look for a link to the full walkthrough, **Then** I find a link to the "Gaze in Practice" blog post for readers who want the complete report with interpretation.
4. **Given** the docs page, **When** I check the existing sections, **Then** none of them have been altered — only a new section has been added.
5. **Given** the sample output section, **When** I compare the data to the blog post, **Then** the numbers are consistent (same project, same report run).

---

### Edge Cases

- What happens if the Gaze output format changes in a future version? Both the blog post and docs excerpt include the Gaze version number (v1.2.3) so readers understand this is a point-in-time snapshot. The blog post date further signals when the output was captured.
- What happens if readers have no context on GazeCRAP or Contract Coverage? The blog post links to the existing "Why Contract Coverage" article for conceptual background. The docs page already explains these metrics above the sample output section.
- What if the gcal-organizer project is not publicly accessible? The blog post focuses on interpreting the output, not on the reader replicating the exact run. The gcal-organizer project is referenced by name but no links to its repository are required.
- What if the report contains sections with warnings (e.g., "0 tests" in quality summary)? The blog post interprets these honestly — they reflect the real state of the project, including its gaps, which is precisely the point of showing real output.

## Requirements _(mandatory)_

### Functional Requirements

#### Blog Post: "Gaze in Practice"

- **FR-001**: The blog post MUST be published as a Markdown file at `content/blog/gaze-in-practice.md` with complete YAML frontmatter including `title`, `description`, `lead`, `slug`, `date`, `draft: false`, `weight`, `toc: true`, `categories`, `tags`, and `contributors`.
- **FR-002**: The article body MUST start with `##` (H2). The `title` frontmatter generates H1.
- **FR-003**: The article MUST reproduce the actual Gaze output from the gcal-organizer project run. Numbers, function names, file paths, grades, and recommendations MUST match the source output exactly. No data may be fabricated or "rounded" for narrative convenience.
- **FR-004**: Each section of the Gaze report (CRAP Summary, Top 5 Worst, GazeCRAP Quadrants, Quality Summary, Classification Summary, Health Assessment, Recommendations) MUST be followed by interpretive narrative explaining what the numbers mean and what actions a developer would consider.
- **FR-005**: The article MUST include a brief introduction establishing context: what project was analyzed, why it was chosen (a real side project, not a synthetic example), and what the reader will learn.
- **FR-006**: The article MUST link to the existing "Why Contract Coverage" blog post for readers who want conceptual background on the metrics.
- **FR-007**: The article MUST link to the Gaze project docs page for readers who want setup instructions or architecture details.
- **FR-008**: All tables MUST use standard Markdown table syntax and render correctly in the Hugo/Doks theme.
- **FR-009**: The article MUST NOT editorialize beyond interpretation. The tone should be "here's what this means" rather than marketing copy.
- **FR-010**: The article MUST honestly present the warnings in the output (e.g., "0 tests" in quality summary, insufficient classification data) without downplaying them.

#### Docs Page: Sample Output Section

- **FR-011**: A new section MUST be added to `content/docs/projects/gaze.md` showing condensed sample output from a real Gaze run.
- **FR-012**: The new section MUST be positioned after the "Three Modes" subsection and before the "Architecture" section — logically placed after explaining what the modes do and before diving into internals.
- **FR-013**: The sample output MUST include at minimum: (a) the Overall Health Assessment scorecard table and (b) the Top 5 Worst CRAP Scores table.
- **FR-014**: The sample output section MUST include a link to the "Gaze in Practice" blog post with a call-to-action like "See the full report walkthrough."
- **FR-015**: The sample output MUST identify the source project and Gaze version so readers understand the data is from a specific real run.
- **FR-016**: The sample output section MUST NOT duplicate the full interpretive narrative from the blog post. It shows the output with minimal annotation — the blog post is for the deep dive.

#### Build and Navigation Integrity

- **FR-017**: The site MUST build successfully with zero errors after all changes.
- **FR-018**: The new blog post MUST appear on the blog index page and in the homepage "Latest Articles" section (it will be the most recent post).
- **FR-019**: All internal links (to "Why Contract Coverage" blog post, to Gaze docs page, between blog post and docs) MUST be valid.
- **FR-020**: The existing "Why Contract Coverage" blog post MUST NOT be altered.
- **FR-021**: All code blocks MUST use fenced syntax with language identifiers (`text` for Gaze output).

### Key Entities

- **Blog Post ("Gaze in Practice")**: A Markdown file in `content/blog/` containing a section-by-section walkthrough of real Gaze output with interpretive narrative. Links to existing conceptual content.
- **Sample Output Section**: A new section in the existing Gaze project docs page showing a condensed excerpt of the same real output, with a link to the full blog walkthrough.
- **Source Output**: The raw Gaze report at `temp/tmp.md`, captured from running `/gaze` on the `gcal-organizer` project (branch `008-decision-extraction`).

### Assumptions

- The Gaze output in `temp/tmp.md` is the authoritative source for all data. No numbers will be modified.
- The gcal-organizer project is a personal side project, which makes it a genuine "real-world" example rather than a demo project built to look good.
- The blog post weight should be lower than the existing "Why Contract Coverage" post (weight 100) so it appears as more recent / listed first on the blog index.
- The homepage "Latest Articles" section (built in spec 003) dynamically sources the most recent blog post, so no homepage changes are needed.
- Emoji characters in the Gaze output (used for grades and section headers) are acceptable in the blog post since they are part of the actual tool output.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: The "Gaze in Practice" blog post is accessible at `/blog/gaze-in-practice/` and renders with all sections, tables, and code blocks correctly formatted.
- **SC-002**: The blog post appears on the blog index page listed above the "Why Contract Coverage" post (more recent date).
- **SC-003**: The blog post appears in the homepage "Latest Articles" section as the most recent post.
- **SC-004**: The Gaze docs page at `/docs/projects/gaze/` includes a new sample output section with the Health Assessment scorecard and Top 5 Worst CRAP Scores table.
- **SC-005**: The sample output section links to the full blog post.
- **SC-006**: All data in both the blog post and docs section exactly matches the source output in `temp/tmp.md`.
- **SC-007**: The site builds successfully with zero errors (`npm run build`).
- **SC-008**: All internal cross-links between the blog post, docs page, and existing "Why Contract Coverage" post are valid.
- **SC-009**: No existing pages are altered except `content/docs/projects/gaze.md` (new section added).
- **SC-010**: Both new and modified pages render correctly in both light and dark mode.
