# Feature Specification: Dewey Documentation

**Feature Branch**: `007-dewey-docs`
**Created**: 2026-03-25
**Status**: Draft
**Input**: User description: "Website Dewey documentation
(Phase 4.4)"

## User Scenarios & Testing _(mandatory)_

### User Story 1 - Dewey Getting Started Guide (Priority: P1)

A developer new to the Unbound Force ecosystem visits
the website to learn how to set up Dewey for knowledge
retrieval. They find a dedicated getting-started page
that walks them through installation, initialization,
source configuration, and first use. After following
the guide, they have a working Dewey instance with
semantic search available to their AI coding agents.

**Why this priority**: Without a getting-started guide,
developers have no documentation for installing and
configuring Dewey. This is the most fundamental page --
it enables adoption.

**Independent Test**: Navigate to the Dewey getting
started page. Verify it covers installation (Homebrew,
model pull), initialization (`dewey init`), source
configuration (YAML examples), indexing (`dewey index`),
and verification (`dewey status`). Verify the page
renders correctly and appears in the sidebar navigation.

**Acceptance Scenarios**:

1. **Given** a visitor on the getting-started section,
   **When** they navigate to the Dewey guide, **Then**
   the page explains what Dewey is, how to install it,
   and how to configure it with step-by-step
   instructions.
2. **Given** a developer reading the Dewey guide,
   **When** they reach the source configuration section,
   **Then** they see YAML examples for local disk,
   GitHub API, and web crawl sources.
3. **Given** a developer reading the Dewey guide,
   **When** they reach the 3-tier degradation section,
   **Then** they understand that Dewey is optional and
   all tools work without it (just with less context).

---

### User Story 2 - Updated Workflow Pages (Priority: P1)

The existing getting-started guides (developer, product
owner, product manager, tester) and the common workflows
page are updated to reference Dewey's role in the hero
lifecycle. Developers reading these pages understand
how Dewey enriches each hero's context and how the
3-tier degradation works in practice.

**Why this priority**: The existing pages describe the
hero workflow but don't mention Dewey or semantic
search. Developers following these guides won't discover
Dewey's capabilities. This is tied with US1 because
both are needed for a coherent documentation experience.

**Independent Test**: Visit each updated page. Verify
it mentions Dewey in the context of the hero's role.
Verify the common workflows page describes how Dewey
provides cross-repo context. Verify no page implies
Dewey is required (degradation is mentioned).

**Acceptance Scenarios**:

1. **Given** a visitor on the developer guide, **When**
   they read the knowledge retrieval section, **Then**
   they see how Cobalt-Crush uses Dewey for toolstack
   docs and implementation patterns.
2. **Given** a visitor on the product owner guide,
   **When** they read the backlog management section,
   **Then** they see how Muti-Mind uses Dewey for
   cross-repo issue discovery.
3. **Given** a visitor on the common workflows page,
   **When** they read the hero lifecycle, **Then** they
   understand that Dewey provides semantic context to
   all heroes during their stages.
4. **Given** a visitor on the tester guide, **When**
   they read the quality analysis section, **Then**
   they see how Gaze uses Dewey for cross-repo quality
   patterns.
5. **Given** a visitor on the product manager guide,
   **When** they read the velocity analysis section,
   **Then** they see how Mx F uses Dewey for cross-repo
   velocity trends and retrospective data.

---

### User Story 3 - Dewey Tool Page (Priority: P2)

A visitor looking at the team section of the website
finds a page for Dewey as an infrastructure tool
alongside the hero pages. The page explains what Dewey
is (a semantic knowledge layer), how it relates to
graphthulhu (its predecessor), what capabilities it
provides (structured + semantic search, content
sources), and how it fits into the ecosystem (used by
all heroes, optional enhancement).

**Why this priority**: The tool page provides a
reference for visitors who want to understand Dewey's
architecture and capabilities in depth. It's P2 because
the getting-started guide (US1) covers the practical
"how to use" angle first.

**Independent Test**: Navigate to the Dewey tool page
in the team section. Verify it explains what Dewey is,
its relationship to graphthulhu, the MCP tools it
provides, content sources, and the embedding model.
Verify it appears in the sidebar navigation under the
team section.

**Acceptance Scenarios**:

1. **Given** a visitor on the team section, **When**
   they navigate to the Dewey page, **Then** they see
   a description of Dewey as a semantic knowledge
   layer that replaces graphthulhu.
2. **Given** a visitor reading the Dewey tool page,
   **When** they look for capabilities, **Then** they
   see both structured search (keyword, tags, wikilinks)
   and semantic search (conceptual similarity).
3. **Given** a visitor reading the Dewey tool page,
   **When** they look for content sources, **Then**
   they see local disk, GitHub API, and web crawl as
   the three source types.

---

### User Story 4 - Navigation Updates (Priority: P2)

The website navigation menus are updated to include the
new Dewey pages. The getting-started section includes
the Dewey guide in the sidebar with appropriate weight
for ordering. The team section includes the Dewey tool
page alongside the hero pages.

**Why this priority**: Without navigation updates, the
new pages exist but are unreachable from the website's
menu structure. This must ship alongside the content.

**Independent Test**: Navigate the website using the
sidebar. Verify the Dewey getting-started guide appears
in the getting-started section. Verify the Dewey tool
page appears in the team section. Verify the ordering
is logical relative to sibling pages.

**Acceptance Scenarios**:

1. **Given** a visitor on the getting-started section,
   **When** they view the sidebar, **Then** the Dewey
   guide appears with a logical position (after the
   role-specific guides, before contributing).
2. **Given** a visitor on the team section, **When**
   they view the sidebar, **Then** the Dewey tool page
   appears alongside the hero pages.

---

### Edge Cases

- What if Dewey is not yet released via Homebrew when
  the docs go live? The getting-started guide should
  include both `brew install` and `go install` as
  installation options, with a note that Homebrew may
  not be available yet.
- What if a visitor reads the Dewey guide before any
  other page? The guide should be self-contained --
  it should explain what Dewey is without assuming
  the reader has read the hero pages or the common
  workflows page.
- What about dark mode rendering? All new pages must
  render correctly in both light and dark mode (Doks
  theme supports auto/light/dark).

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: The website MUST include a Dewey
  getting-started page at
  `content/docs/getting-started/knowledge.md` covering
  installation, initialization, source configuration,
  indexing, and verification.
- **FR-002**: The Dewey getting-started page MUST
  include YAML configuration examples for all three
  source types (local disk, GitHub API, web crawl).
- **FR-003**: The Dewey getting-started page MUST
  explain the 3-tier graceful degradation (full Dewey,
  graph-only, no Dewey) so developers understand Dewey
  is optional.
- **FR-004**: The website MUST include a Dewey tool page
  at `content/docs/team/dewey.md` describing Dewey's
  role, capabilities, and relationship to graphthulhu.
- **FR-005**: The common workflows page MUST be updated
  to reference Dewey's role in providing semantic
  context to hero agents during the workflow.
- **FR-006**: The developer guide MUST be updated to
  describe how Cobalt-Crush uses Dewey for toolstack
  documentation and implementation patterns.
- **FR-007**: The product owner guide MUST be updated
  to describe how Muti-Mind uses Dewey for cross-repo
  backlog patterns and issue discovery.
- **FR-008**: The product manager guide MUST be updated
  to describe how Mx F uses Dewey for cross-repo
  velocity trends and retrospective data.
- **FR-009**: The tester guide MUST be updated to
  describe how Gaze uses Dewey for cross-repo quality
  patterns and test baselines.
- **FR-010**: The sidebar navigation MUST include the
  new Dewey pages with appropriate ordering.
- **FR-011**: All new and updated pages MUST render
  correctly in both light and dark mode.
- **FR-012**: All new pages MUST follow the website's
  frontmatter conventions (title, description, lead,
  date, weight, toc, draft).
- **FR-013**: The site MUST build without errors after
  all changes (`npm run build`).
- **FR-014**: The getting-started section index
  (`content/docs/getting-started/_index.md`) MUST be
  updated to include a link to the Dewey knowledge
  retrieval guide.
- **FR-015**: The team section index
  (`content/docs/team/_index.md`) MUST be updated to
  include Dewey as an infrastructure tool, distinct
  from the hero personas.

## Assumptions

- The Dewey design paper
  (`../dewey-design-paper.md`) and architectural spec
  (Spec 014 in the meta repo) are the primary content
  sources. Content should be adapted for a website
  audience -- less raw technical detail, more "why
  should I care" framing.
- The existing page style and tone (conversational,
  developer-focused, practical) should be maintained
  for consistency.
- The Dewey binary may or may not be available via
  Homebrew when the docs go live. Installation
  instructions should include both Homebrew and
  `go install` options.
- The getting-started guide is the primary entry point
  for Dewey documentation. The tool page is a deeper
  reference.
- No changes to the homepage layout (`layouts/home.html`)
  are needed -- Dewey is not a project, it's
  infrastructure.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: The website has a dedicated Dewey
  getting-started page reachable from the sidebar
  navigation with installation, configuration, and
  usage instructions.
- **SC-002**: All 4 role-specific guides (developer,
  product owner, product manager, tester) reference
  Dewey's role in their hero's workflow.
- **SC-003**: The common workflows page describes
  Dewey's role in the hero lifecycle.
- **SC-004**: `npm run build` succeeds with zero
  errors after all changes.
- **SC-005**: All new and updated pages have readable
  code blocks (YAML examples), visible table borders,
  and no text-on-same-color-background issues in both
  light and dark mode (verified via `npm run dev` with
  manual inspection of each page in both modes).
- **SC-006**: Zero broken internal links across all
  new and updated pages (verified by clicking each
  link in the dev server and confirming no 404s).
