# Feature Specification: Documentation Content Pages

**Feature Branch**: `002-documentation-content`
**Created**: 2026-02-23
**Status**: Draft
**Input**: User description: "Create all documentation sections with real content adapted from source repositories: Getting Started, Projects (Gaze), Team (5 agent personas), and Contributing."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Getting Started Guide (Priority: P1)

As a first-time visitor who clicked "Get Started" on the homepage, I want to understand what Unbound Force is and how to begin using it so that I can evaluate whether it fits my needs.

**Why this priority**: This is the primary conversion path from the homepage CTA. Without it, the "Get Started" button leads nowhere.

**Independent Test**: Click "Get Started" on the homepage. The Getting Started section loads with a clear explanation of Unbound Force and actionable steps to begin.

**Acceptance Scenarios**:

1. **Given** I click "Get Started" on the homepage, **When** the page loads, **Then** I see the "What is Unbound Force?" page with a clear explanation adapted from the `unbound-force/unbound-force.md` overview.
2. **Given** I am on the Getting Started index page, **When** I look at the sidebar, **Then** I see a "Quick Start" sub-page that explains how to get the tools running (Specify, OpenCode, Swarm).
3. **Given** the Getting Started content, **When** I cross-reference it against the `unbound-force/unbound-force.md` source, **Then** the content is accurate but adapted for a website audience (less raw technical detail, more "why should I care" framing).
4. **Given** the Getting Started section, **When** I check the frontmatter of each page, **Then** all required fields are present (`title`, `description`, `lead`, `date`, `draft`, `weight`, `toc`).

---

### User Story 2 - Gaze Project Page (Priority: P1)

As a developer interested in test quality, I want to read about the Gaze project so that I understand what it does, how to install it, and whether it solves my problem.

**Why this priority**: Gaze is the flagship project. The homepage links to it, and it's the primary concrete offering from Unbound Force.

**Independent Test**: Navigate to the Gaze project page. The page explains what Gaze does, how to install it, its key commands, and its current limitations.

**Acceptance Scenarios**:

1. **Given** I navigate to the Projects section, **When** I see the projects overview page, **Then** it lists available projects with brief descriptions.
2. **Given** I click on the Gaze project, **When** the page loads, **Then** I see a description of Gaze covering: what it does (side effect detection + CRAP scores for Go), installation (`go install`), key commands (`gaze analyze`, `gaze crap`), output formats (text + JSON), and architecture overview.
3. **Given** the Gaze project page content, **When** I cross-reference it against the Gaze repository README, **Then** all facts are accurate — no fabricated features, no overstated capabilities.
4. **Given** the Gaze project page, **When** I look for known limitations, **Then** the page honestly lists current limitations (direct function body only, P3-P4 not yet detected, line coverage fallback, no CGo/unsafe, single package for analyze).
5. **Given** I am on the Gaze project page, **When** I check for a link to the Gaze repository, **Then** a prominent link to `https://github.com/unbound-force/gaze` is present.

---

### User Story 3 - Team Persona Pages (Priority: P2)

As a visitor curious about how the AI agent swarm works, I want to read about each agent persona so that I understand the team structure and each role's purpose.

**Why this priority**: The team personas are a differentiator for Unbound Force, but they're explanatory content rather than actionable tooling. Visitors can understand UF without them.

**Independent Test**: Navigate to the Team section. The overview lists all 5 personas. Each persona page loads with a complete, accurate description.

**Acceptance Scenarios**:

1. **Given** I navigate to the Team section, **When** I see the overview page, **Then** it introduces "The Heroes" concept and lists all 5 agent personas with brief descriptions.
2. **Given** I click on Muti-Mind, **When** the page loads, **Then** I see a description of the Product Owner persona covering vision keeping, prioritization, backlog ownership, and acceptance authority — adapted from `unbound-force.md`.
3. **Given** I click on Cobalt-Crush, **When** the page loads, **Then** I see a description of the Developer persona covering high-velocity implementation, CI/CD focus, architectural adherence, and tight feedback loops.
4. **Given** I click on Gaze (Tester), **When** the page loads, **Then** I see a description of the Tester persona covering proactive test strategies, CI/CV pipelines, intelligent defect detection, and risk-based testing.
5. **Given** I click on The Divisor, **When** the page loads, **Then** I see a description of the PR Reviewer persona covering the three-persona council (The Guard, The Architect, The Adversary) and collective merge authority.
6. **Given** I click on Mx F, **When** the page loads, **Then** I see a description of the Manager persona covering servant leadership, flow facilitation, continuous improvement coaching, and obstacle removal.
7. **Given** any persona page, **When** I cross-reference the content against `unbound-force.md`, **Then** the descriptions are accurate but adapted for a website audience.

---

### User Story 4 - Contributing Guide (Priority: P2)

As a developer who wants to contribute to Unbound Force projects, I want to find clear contributing guidelines so that I know how to submit changes.

**Why this priority**: Contributing guidelines are important for community building but are not needed for initial site launch. Visitors can contribute via GitHub without a dedicated page.

**Independent Test**: Navigate to the Contributing section. The page explains how to contribute with clear, actionable steps.

**Acceptance Scenarios**:

1. **Given** I navigate to the Contributing section, **When** the page loads, **Then** I see clear instructions covering: how to report issues, how to submit pull requests, coding conventions (Conventional Commits), and the Speckit workflow requirement.
2. **Given** the Contributing page, **When** I look for links, **Then** I find links to the GitHub organization and relevant repositories.

---

### User Story 5 - Navigation Across All Sections (Priority: P1)

As a visitor browsing the site, I want all documentation sections to appear in the sidebar navigation in a logical order so that I can find any page within two clicks from the homepage.

**Why this priority**: Navigation is critical to Visitor Clarity (Constitution Principle III). Broken or missing navigation makes content undiscoverable.

**Independent Test**: From the homepage, verify that every documentation page is reachable within two clicks and appears in the sidebar in the correct order.

**Acceptance Scenarios**:

1. **Given** I am on any page, **When** I look at the sidebar, **Then** I see all four sections (Getting Started, Projects, Team, Contributing) in that order.
2. **Given** I am in the Getting Started section, **When** I look at the sidebar, **Then** I see the section index and Quick Start sub-page in the correct order.
3. **Given** I am in the Team section, **When** I look at the sidebar, **Then** I see the overview followed by all 5 persona pages in a logical order.
4. **Given** the navigation menu configuration, **When** I inspect `menus.en.toml`, **Then** `[[docs]]` entries define all sections with correct `weight` values for ordering.
5. **Given** all content pages, **When** I run `npm run build`, **Then** the build succeeds with no errors, no orphaned pages, and no broken internal links.

---

### Edge Cases

- What happens if a persona description in `unbound-force.md` is updated upstream? The website page must be manually updated to stay in sync (no automated content sync in v1).
- What happens if the Gaze README adds a new command? The Gaze project page must be manually updated.
- What if a content page is added without updating `menus.en.toml`? It will appear in the sidebar via Hugo's automatic section detection but may not appear in the top nav. All pages must be verified against navigation.
- What if two pages have the same `weight` value? Hugo uses alphabetical ordering as a tiebreaker, which may produce unexpected sidebar ordering. Each page must have a unique weight within its section.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The Getting Started section MUST contain an index page (`content/docs/getting-started/_index.md`) explaining what Unbound Force is, adapted from the `unbound-force/unbound-force.md` overview section.
- **FR-002**: The Getting Started section MUST contain a Quick Start page (`content/docs/getting-started/quick-start.md`) explaining how to get started with the tools (Specify, OpenCode, and optionally Swarm).
- **FR-003**: The Projects section MUST contain an index page (`content/docs/projects/_index.md`) with a brief overview of available projects.
- **FR-004**: The Projects section MUST contain a Gaze project page (`content/docs/projects/gaze.md`) adapted from the Gaze repository README, covering: description, installation, key commands (`gaze analyze`, `gaze crap`), output formats, architecture overview, and known limitations.
- **FR-005**: The Gaze project page MUST NOT fabricate features or omit known limitations. Content MUST be cross-referenced against the Gaze README.
- **FR-006**: The Team section MUST contain an index page (`content/docs/team/_index.md`) introducing the "Heroes" concept and listing all 5 personas.
- **FR-007**: The Team section MUST contain individual pages for each persona: `muti-mind.md` (Product Owner), `cobalt-crush.md` (Developer), `gaze-tester.md` (Tester), `the-divisor.md` (PR Reviewer), `mx-f.md` (Manager).
- **FR-008**: Each persona page MUST be adapted from the `unbound-force/unbound-force.md` Heroes section. Content MUST be rewritten for a website audience — not copied verbatim.
- **FR-009**: The Divisor page MUST describe the three sub-personas (The Guard, The Architect, The Adversary) and the collective approval requirement.
- **FR-010**: The Contributing section MUST contain an index page (`content/docs/contributing/_index.md`) with guidelines for reporting issues, submitting PRs, following Conventional Commits, and using the Speckit workflow.
- **FR-011**: Every content page MUST include all required YAML frontmatter fields: `title`, `description`, `lead`, `date`, `draft: false`, `weight`, `toc: true`.
- **FR-012**: Every content page MUST start body content with `##` (H2). H1 MUST NOT appear in body content.
- **FR-013**: Section index pages MUST be named `_index.md` (with leading underscore).
- **FR-014**: The `config/_default/menus/menus.en.toml` MUST be updated to include all documentation sections with correct `weight` values for sidebar ordering: Getting Started (10), Projects (20), Team (30), Contributing (40).
- **FR-015**: All internal links between pages MUST be valid. No broken links or references to non-existent pages.
- **FR-016**: Content MUST be adapted for a website audience. Raw README content MUST NOT be copied verbatim. Frame content around "why should I care" rather than raw technical setup details.

### Key Entities

- **Content Page**: A Markdown file in `content/docs/` with YAML frontmatter and body content. Each page has a title, description, weight (for ordering), and belongs to a section.
- **Section Index**: A `_index.md` file that serves as the landing page for a documentation section. Controls section title and provides overview content.
- **Navigation Menu**: The `menus.en.toml` configuration that defines sidebar sections, top nav links, and footer links.
- **Source Repository**: An upstream Unbound Force repository (Gaze, unbound-force) from which website content is derived and must be kept in sync.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: All 14 content pages exist and render without errors: 2 Getting Started, 2 Projects, 6 Team (1 index + 5 personas), 1 Contributing, plus the docs-level `_index.md`.
- **SC-002**: Every page is reachable from the homepage within two clicks via sidebar navigation.
- **SC-003**: `npm run build` succeeds with zero errors after all content is added.
- **SC-004**: Cross-referencing the Gaze project page against the Gaze README confirms all facts are accurate and no features are fabricated.
- **SC-005**: Cross-referencing each persona page against `unbound-force.md` confirms all descriptions are accurate and properly adapted.
- **SC-006**: No page contains placeholder content ("Coming Soon", "TODO", "TBD", or similar).
- **SC-007**: All content pages have complete YAML frontmatter with all required fields populated.
- **SC-008**: Sidebar navigation shows all sections in the correct order: Getting Started, Projects, Team, Contributing.
