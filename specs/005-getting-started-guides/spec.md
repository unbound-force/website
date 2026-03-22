# Feature Specification: Getting Started Guides

**Feature Branch**: `005-getting-started-guides`
**Created**: 2026-03-21
**Status**: Draft
**Input**: User description: "Add role-specific getting started guides for developers, testers, product owners, and product managers, plus a common workflows cross-role reference page."

## User Scenarios & Testing _(mandatory)_

### User Story 1 - Developer Getting Started Guide (Priority: P1)

A new developer arrives at the Unbound Force project and
needs to understand how to set up their environment, use
the daily workflow, and leverage the Cobalt-Crush persona
for coding. They visit the getting started section of the
website and find a dedicated developer guide that walks
them through prerequisites, daily workflow, Speckit
pipeline, Swarm coordination, and session lifecycle.

**Why this priority**: Developers are the primary audience
for Unbound Force. Without a developer guide, no one can
productively use the tools. This is the most critical
guide because it covers the largest tool surface (OpenCode,
Swarm, Speckit, convention packs) and the most complex
workflows.

**Independent Test**: Can be fully tested by navigating to
the getting started section, clicking the developer guide,
and verifying all sections render with actionable content
covering prerequisites, daily workflow, Speckit, Swarm,
and session lifecycle.

**Acceptance Scenarios**:

1. **Given** a visitor on the getting started section,
   **When** they click on the developer guide, **Then**
   they see a page with sections covering: prerequisites
   (`unbound setup`), daily workflow
   (`unbound doctor` then OpenCode then Swarm), Speckit
   pipeline (specify through implement), Swarm usage
   (`/swarm`, file reservations, session lifecycle),
   Cobalt-Crush persona and convention packs, and session
   ritual (hive_ready through git push).

2. **Given** a developer reading the prerequisites
   section, **When** they follow the instructions,
   **Then** they can run `unbound setup` as a single
   command to install all required tools.

3. **Given** a developer reading the Swarm section,
   **When** they follow the session lifecycle guidance,
   **Then** they understand the start (hive_ready), work
   (/swarm), and end (hive_sync + git push) ritual.

---

### User Story 2 - Common Workflows Reference (Priority: P1)

A team member of any role needs a quick reference for how
common end-to-end workflows operate across roles. They
visit the common workflows page and find step-by-step
flows for new features, bug fixes, code reviews, and
environment setup.

**Why this priority**: This guide ties together all the
role-specific guides into a cohesive cross-role picture.
Without it, each role operates in isolation without
understanding how their work flows into the next stage.
Equal priority with the developer guide because it serves
all roles.

**Independent Test**: Can be fully tested by navigating to
the common workflows page and verifying it covers 4
workflows: new feature (end-to-end with all roles), bug
fix (OpenSpec tactical), code review (review council), and
environment setup.

**Acceptance Scenarios**:

1. **Given** a visitor on the common workflows page,
   **When** they read the new feature workflow, **Then**
   they see the full end-to-end flow: PO defines, Dev
   specs, Dev implements, Tester validates, Reviewer
   approves, PO accepts, PM measures.

2. **Given** a visitor reading the bug fix workflow,
   **When** they follow the steps, **Then** they
   understand the OpenSpec tactical flow: propose, apply,
   review, archive.

3. **Given** a visitor reading the code review workflow,
   **When** they follow the steps, **Then** they
   understand how to invoke `/review-council` and what the
   5 Divisor personas evaluate.

4. **Given** a visitor reading the environment setup
   workflow, **When** they follow the steps, **Then** they
   understand the progression: `unbound setup`, then
   `unbound doctor`, then their first task.

---

### User Story 3 - Tester Getting Started Guide (Priority: P2)

A tester or quality engineer arrives at the project and
needs to understand how to use Gaze for code quality
analysis, interpret quality metrics, and integrate with
the hero workflow and CI pipelines.

**Why this priority**: Testers are the second-most
technical role and Gaze is the most mature standalone tool.
This guide bridges the gap between Gaze's CLI documentation
(on the Gaze project page) and the Unbound Force workflow
context.

**Independent Test**: Can be fully tested by navigating to
the tester guide and verifying sections cover Gaze
commands, quality metrics, the review council, and CI
integration.

**Acceptance Scenarios**:

1. **Given** a visitor on the tester guide, **When** they
   read the Gaze section, **Then** they see coverage of
   `gaze analyze`, `gaze quality`, `gaze crap`, and
   `gaze report` with brief descriptions of what each
   command produces.

2. **Given** a visitor reading the quality metrics
   section, **When** they review the content, **Then**
   they understand contract coverage, side effects, and
   CRAP scores at a conceptual level.

3. **Given** a visitor reading the CI section, **When**
   they review the content, **Then** they understand how
   coverage ratchets work and how Gaze integrates with CI
   pipelines.

---

### User Story 4 - Product Owner Getting Started Guide (Priority: P2)

A product owner or business stakeholder needs to understand
how to use Muti-Mind for backlog management, how to provide
acceptance criteria for specifications, and how their
decisions flow through the hero lifecycle.

**Why this priority**: Product owners are essential to the
hero workflow (define and accept stages). Without PO
engagement, the workflow cannot start or end properly.

**Independent Test**: Can be fully tested by navigating to
the product owner guide and verifying sections cover
Muti-Mind usage, the hero lifecycle from a PO perspective,
and working with specifications.

**Acceptance Scenarios**:

1. **Given** a visitor on the product owner guide,
   **When** they read the Muti-Mind section, **Then** they
   understand backlog management, priority scoring, and
   acceptance decisions at a workflow level.

2. **Given** a visitor reading the hero lifecycle section,
   **When** they review the content, **Then** they
   understand the PO's role in the define (stage 1) and
   accept (stage 5) stages, including what artifacts they
   receive and produce.

3. **Given** a visitor reading the specifications section,
   **When** they review the content, **Then** they
   understand how to read a Speckit spec and how to
   provide acceptance criteria.

---

### User Story 5 - Product Manager Getting Started Guide (Priority: P3)

A product manager or engineering manager needs to
understand how to use Mx F for sprint management, read
dashboards, track impediments, and run retrospectives.

**Why this priority**: Product managers interact primarily
with metrics and coaching tools, which are secondary to
the core development and testing workflows. This guide is
valuable but serves a smaller audience.

**Independent Test**: Can be fully tested by navigating to
the product manager guide and verifying sections cover
Mx F commands, dashboard reading, coaching, and retros.

**Acceptance Scenarios**:

1. **Given** a visitor on the product manager guide,
   **When** they read the Mx F section, **Then** they
   understand sprint management, metrics dashboards,
   impediment tracking, and coaching at a workflow level.

2. **Given** a visitor reading the dashboard section,
   **When** they review the content, **Then** they
   understand what velocity, quality trends, and hero
   utilization metrics represent.

3. **Given** a visitor reading the integration section,
   **When** they review the content, **Then** they
   understand how Mx F metrics relate to Muti-Mind
   backlog items and Gaze quality reports.

---

### Edge Cases

- What happens when a guide references a tool that is not
  yet released or documented? The guide MUST link to the
  relevant project page on the website if it exists, or
  to the GitHub repository if no project page exists. The
  guide MUST NOT describe features that do not yet exist.

- What happens when a guide references commands or
  workflows that span multiple roles? The guide SHOULD
  link to the Common Workflows page rather than
  duplicating the cross-role flow.

- What happens when dark mode is active? All guide pages
  MUST render correctly in both light and dark mode,
  following the existing Doks theme conventions.

- What happens when the getting started section already
  has content (e.g., quick-start.md)? The new guides
  MUST coexist with the existing quick-start page without
  breaking navigation. The sidebar weight ordering MUST
  place the existing quick-start page first, followed by
  the role-specific guides, then common workflows.

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: The website MUST include a "Developer /
  Engineer" getting started guide page accessible from the
  documentation sidebar under the "Getting Started"
  section.

- **FR-002**: The website MUST include a "Tester" getting
  started guide page accessible from the documentation
  sidebar under the "Getting Started" section.

- **FR-003**: The website MUST include a "Product Owner"
  getting started guide page accessible from the
  documentation sidebar under the "Getting Started"
  section.

- **FR-004**: The website MUST include a "Product Manager"
  getting started guide page accessible from the
  documentation sidebar under the "Getting Started"
  section.

- **FR-005**: The website MUST include a "Common Workflows"
  reference page accessible from the documentation sidebar
  under the "Getting Started" section.

- **FR-006**: Each guide page MUST include valid Hugo
  frontmatter with title, description, lead, date, weight,
  and toc fields, following the conventions defined in
  AGENTS.md.

- **FR-007**: Each guide page MUST start body content with
  H2 headings (the title frontmatter generates H1),
  following the markdown rules defined in AGENTS.md.

- **FR-008**: Guide pages MUST use `weight` values that
  order them in the sidebar after the existing quick-start
  page: quick-start (existing weight), then Developer,
  Tester, Product Owner, Product Manager, Common Workflows.

- **FR-009**: Each guide MUST contain only real, accurate
  content derived from actual tool capabilities. No
  placeholder or "Coming Soon" content, per the zero-waste
  mandate in AGENTS.md.

- **FR-010**: Each guide MUST render correctly in both
  light and dark mode.

- **FR-011**: The developer guide MUST cover these topics:
  prerequisites (unbound setup), daily workflow (doctor
  then OpenCode then Swarm), Speckit pipeline (specify
  through implement), Swarm coordination (/swarm, file
  reservations, session lifecycle), Cobalt-Crush persona
  and convention packs, session ritual (hive_ready through
  git push).

- **FR-012**: The tester guide MUST cover these topics:
  Gaze commands (analyze, quality, crap, report), quality
  metrics (contract coverage, side effects, CRAP scores),
  how Gaze feeds into the hero workflow, the review council
  (/review-council), coverage ratchets and CI integration.

- **FR-013**: The product owner guide MUST cover these
  topics: Muti-Mind usage (backlog management, priority
  scoring, acceptance decisions), GitHub issue sync, the
  PO role in the 6-stage hero lifecycle, working with
  specifications (reading specs, providing acceptance
  criteria).

- **FR-014**: The product manager guide MUST cover these
  topics: Mx F usage (sprint management, metrics
  dashboards, impediment tracking), coaching and
  retrospectives, how Mx F metrics integrate with
  Muti-Mind and Gaze, reading the dashboard (velocity,
  quality trends, hero utilization).

- **FR-015**: The common workflows page MUST cover these
  workflows: new feature (end-to-end across all roles),
  bug fix (OpenSpec tactical flow), code review
  (/review-council with 5 Divisor personas), environment
  setup (unbound setup then doctor then first task).

- **FR-016**: Cross-references between guides MUST use
  relative links that work in the built site.

- **FR-017**: The site MUST build successfully with
  `npm run build` after adding all guide pages.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: All 5 guide pages are accessible from the
  documentation sidebar under "Getting Started" and render
  without errors in both light and dark mode.

- **SC-002**: Each guide page contains at least 3 distinct
  content sections with actionable guidance (not
  placeholder text).

- **SC-003**: The site builds successfully with
  `npm run build` with zero errors after adding all pages.

- **SC-004**: A developer who has never used Unbound Force
  can follow the developer guide from top to bottom and
  understand the complete workflow from setup to session
  end without consulting any other documentation.

- **SC-005**: The common workflows page covers all 4
  specified workflows with step-by-step flows that
  identify which role is responsible at each step.

- **SC-006**: All cross-references between guides resolve
  to valid pages (no broken internal links).

### Assumptions

- Content is sourced from the Unbound Force project's
  actual tool capabilities (README files, AGENTS.md, spec
  artifacts, and upstream documentation at swarmtools.ai
  and opencode.ai).

- The guides are written for a technical audience
  (developers, testers, product owners, product managers)
  who are familiar with software development concepts but
  not with Unbound Force specifically.

- The Doks/Thulite theme's built-in sidebar navigation
  automatically discovers pages in `content/docs/` based
  on directory structure and frontmatter weight.

- The existing quick-start page remains unchanged. The new
  guides complement it rather than replacing it.

### Dependencies

- The existing `content/docs/getting-started/` section and
  its `_index.md` file.

- Accurate content about Gaze, Muti-Mind, Mx F, Swarm,
  OpenCode, and Speckit sourced from upstream repositories
  and documentation.

- The Doks/Thulite theme's sidebar rendering behavior for
  ordering pages by weight.
