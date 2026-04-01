# Feature Specification: Unleash Visibility

**Feature Branch**: `013-unleash-visibility`
**Created**: 2026-03-31
**Status**: Ready
**Input**: User description: "Surface /unleash as the flagship capability across the website. Implement all recommendations from the tech writer/PR review: blog post, homepage/Quick Start/developer guide cross-references, common-workflows restructuring, and homepage CTA update."

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Blog Post: From Spec to Demo in One Command (Priority: P1)

A developer discovers the Unbound Force blog through search or social sharing. They read a post that walks through a real `/unleash` run -- from a 1-sentence seed to demo-ready code -- showing the actual 8-step pipeline with terminal output at each stage. The post explains how Dewey auto-clarifies questions, how Swarm runs parallel workers, and how the Divisor review council gates quality. By the end, the reader understands the full autonomous workflow and has a concrete command they can run immediately.

**Why this priority**: A blog post is the highest-PR-value deliverable. It can be shared on Hacker News, dev.to, and social media independently of the rest of the website. The two existing blog posts (Gaze-focused) are the site's strongest content -- an `/unleash` post would broaden the audience from Go-testing enthusiasts to the entire AI-assisted development community. Blog posts also drive SEO, providing a durable discovery path.

**Independent Test**: Navigate to the blog page and verify the post exists, renders correctly, includes a complete walkthrough of the 8-step pipeline, and has a clear call-to-action linking to the Quick Start page.

**Acceptance Scenarios**:

1. **Given** a visitor on the blog index page, **When** they browse the latest articles, **Then** they see a post about the autonomous pipeline that explains what `/unleash` does in its title and description.
2. **Given** a reader of the blog post, **When** they read the walkthrough, **Then** each of the 8 pipeline steps is explained with a description of what happens at that stage.
3. **Given** a reader of the blog post, **When** they look for how to get started, **Then** they find a clear path: install instructions and a link to the Quick Start page.
4. **Given** a reader of the blog post, **When** they look for exit points, **Then** they understand where the pipeline pauses for human judgment and how to resume with a re-run.

---

### User Story 2 — Surface /unleash on Entry-Point Pages (Priority: P1)

A first-time visitor navigates through the site's entry-point pages (homepage, Quick Start, Developer Guide, Getting Started landing, Product Owner guide). Currently, none of these pages mention `/unleash`. After this change, every primary entry path surfaces the autonomous pipeline capability, ensuring visitors discover `/unleash` regardless of which page they land on first.

**Why this priority**: The audit found that `/unleash` appears on 1 of 22 pages. This is the core discovery problem -- adding cross-references to 5 high-traffic entry pages immediately multiplies visibility. Combined with the blog post (US1), this ensures both external discovery (blog/search) and internal discovery (site navigation) are covered.

**Independent Test**: Navigate to the homepage, Quick Start, Developer Guide, Getting Started landing page, and Product Owner guide. On each page, search for "unleash" and verify it appears with a link to the full documentation.

**Acceptance Scenarios**:

1. **Given** a visitor on the homepage, **When** they read the feature cards in the "Why Unbound Force?" section, **Then** they find the autonomous pipeline capability mentioned prominently, with `/unleash` named.
2. **Given** a new user on the Quick Start page, **When** they reach the "Start Working" section, **Then** `/unleash` is presented as the first and simplest option, with `/speckit.specify` offered for manual control.
3. **Given** a developer reading the Developer Guide, **When** they reach the Speckit pipeline section, **Then** they find a prominent note at the top indicating `/unleash` runs the entire pipeline autonomously, before the individual command listing.
4. **Given** a visitor on the Getting Started landing page, **When** they read the Common Workflows description, **Then** it surfaces the autonomous pipeline capability rather than a generic description.
5. **Given** a product owner reading their guide, **When** they read about the swarm's autonomous workflow, **Then** `/unleash` is named as the command alongside `/workflow seed`.

---

### User Story 3 — Restructure Common Workflows Page (Priority: P2)

A developer opens the Common Workflows page to learn how to work with the swarm. Currently, `/unleash` is the 3rd of 7 sections, buried 277 lines deep behind the entire manual 6-stage lifecycle and the bug fix workflow. After restructuring, the autonomous pipeline is the first thing they see -- with the manual workflow presented as an alternative for when fine-grained control is needed.

**Why this priority**: This is the only page that documents `/unleash`, so its position within the page determines discoverability for anyone who does navigate here. The restructuring reinforces the message from US2's cross-references: the autonomous pipeline is the recommended path, not a footnote.

**Independent Test**: Open common-workflows.md and verify that the Autonomous Pipeline section appears before the manual New Feature section, and that the page narrative flows from "easiest path" to "manual control" to "tactical fixes."

**Acceptance Scenarios**:

1. **Given** a developer opening the Common Workflows page, **When** they start reading, **Then** the first workflow section they encounter is the Autonomous Pipeline (`/unleash`), not the manual 6-stage lifecycle.
2. **Given** a developer reading the restructured page, **When** they look at the table of contents, **Then** the section order communicates a clear narrative: autonomous pipeline first, end-of-branch workflow second, manual feature workflow third, bug fix workflow fourth, code review fifth, environment setup last.
3. **Given** a developer who wants manual control, **When** they scroll past the `/unleash` section, **Then** they find the manual New Feature workflow with a note indicating it is the step-by-step alternative to `/unleash`.

---

### User Story 4 — Homepage CTA and Feature Card Update (Priority: P2)

A visitor lands on the homepage and sees a compelling description of the autonomous pipeline capability alongside a direct call-to-action. Currently, the homepage's "Speckit Workflow" feature card describes a multi-step manual process, and the CTA sends visitors to either GitHub (code repo) or the Quick Start (generic setup). After this change, the homepage communicates the autonomous capability and provides a clear path to try it.

**Why this priority**: The homepage is the highest-traffic page and sets the visitor's first impression. Currently it undersells the system's most compelling capability. This change is lower priority than the blog post (US1) and cross-references (US2) because those create new discovery paths, while this optimizes an existing one.

**Independent Test**: Visit the homepage and verify: (a) the feature card section mentions autonomous pipeline capability, (b) the bottom CTA section invites visitors to try the swarm with a link to the Quick Start.

**Acceptance Scenarios**:

1. **Given** a visitor on the homepage, **When** they read the "Why Unbound Force?" feature cards, **Then** the Speckit Workflow card or a new card communicates that one command takes a spec from draft to demo-ready code, not just that a structured pipeline exists.
2. **Given** a visitor who reaches the bottom CTA, **When** they look for how to get started, **Then** they find a primary action that leads to the Quick Start (where `/unleash` is now the first option), not just to GitHub.

---

### Edge Cases

- What happens when the blog post references pipeline stages that are documented in common-workflows.md? The blog post must link to the detailed documentation rather than duplicating it. The post should be self-contained enough to understand the concept but link out for reference details.
- What happens when a reader arrives at the old common-workflows URL with an anchor link to the manual New Feature section? Anchor links change when sections are reordered. The manual New Feature section should keep the same anchor slug (`#new-feature-end-to-end`) despite its new position in the page to avoid breaking external links.
- What happens when `/unleash` behavior changes in a future version? The blog post should describe the current capability without version-coupling the narrative. Use present tense and avoid specific version numbers in the post.
- What happens when a visitor reads the homepage feature card but doesn't have the swarm installed? The card should describe the capability without assuming installation. The CTA should point to Quick Start, which handles installation first.

## Requirements _(mandatory)_

### Functional Requirements

**Blog Post (US1):**

- **FR-001**: The website MUST have a blog post that walks through the `/unleash` pipeline from a feature seed to demo-ready output.
- **FR-002**: The blog post MUST describe all 8 pipeline steps (clarify, plan, tasks, spec review, implement, code review, retrospective, demo) with what happens at each stage.
- **FR-003**: The blog post MUST explain exit points -- where the pipeline pauses for human judgment -- and how resuming works.
- **FR-004**: The blog post MUST describe the key capabilities that make `/unleash` autonomous: Dewey-powered clarification, parallel Swarm workers, Divisor review council, Hivemind learning storage.
- **FR-005**: The blog post MUST include a "Getting Started" section with install instructions and a link to the Quick Start page.
- **FR-006**: The blog post MUST follow the existing blog post conventions: YAML frontmatter with title, description, lead, slug, date, categories, tags, contributors; body starting with H2; fenced code blocks with language identifiers.

**Entry-Point Cross-References (US2):**

- **FR-007**: The homepage feature cards MUST mention the autonomous pipeline capability with `/unleash` named.
- **FR-008**: The Quick Start "Start Working" section MUST present `/unleash` as the first and simplest option, followed by `/speckit.specify` for manual control.
- **FR-009**: The Developer Guide Speckit section MUST include a prominent note indicating `/unleash` runs the entire pipeline autonomously, placed before the individual command listing.
- **FR-010**: The Getting Started landing page MUST update the Common Workflows description to surface the autonomous pipeline.
- **FR-011**: The Product Owner guide MUST name `/unleash` alongside `/workflow seed` when describing the swarm's autonomous workflow.

**Common Workflows Restructuring (US3):**

- **FR-012**: The common-workflows page MUST place the Autonomous Pipeline (`/unleash`) section before the manual New Feature (End-to-End) section.
- **FR-013**: The End-of-Branch Workflow (`/finale`) section MUST immediately follow the Autonomous Pipeline section (unleash builds, finale ships).
- **FR-014**: The manual New Feature section MUST include a note at the top indicating it is the step-by-step alternative to `/unleash` for when fine-grained control is needed.
- **FR-015**: The manual New Feature section MUST preserve the anchor slug `#new-feature-end-to-end` to avoid breaking external links.

**Homepage CTA (US4):**

- **FR-016**: The homepage Speckit Workflow feature card MUST communicate that one command takes a spec from draft to demo-ready code.
- **FR-017**: The homepage bottom CTA MUST lead visitors to the Quick Start page as the primary action.

**Developer Guide Daily Workflow (US2, clarification):**

- **FR-020**: The Developer Guide "Daily Workflow" section MUST be rewritten to center on the specify -> unleash -> (clarify if needed) -> finale loop, replacing the current generic session flow (`uf doctor`, `hive_ready()`, `/swarm`, `hive_sync()`, `git push`).

**Cross-cutting:**

- **FR-018**: The blog post MUST cross-link to common-workflows for detailed pipeline documentation rather than duplicating reference content.
- **FR-019**: All documentation changes MUST pass `npm run build` without errors.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: `/unleash` is mentioned on at least 7 user-facing pages (blog post + homepage + Quick Start + Developer Guide + Getting Started landing + Product Owner guide + common-workflows) -- up from 1 page currently.
- **SC-002**: A visitor starting from the homepage can discover `/unleash` documentation within 2 clicks (homepage -> Quick Start -> `/unleash` is first option).
- **SC-003**: The blog post exists at a discoverable URL (`/blog/unleash-in-practice/`) and its frontmatter `title` and `description` contain the terms "autonomous," "pipeline," and "spec to code" for search relevance.
- **SC-004**: The Autonomous Pipeline section appears before the manual New Feature section in common-workflows.md -- visible without scrolling past other workflow documentation.
- **SC-005**: `npm run build` succeeds with zero errors after all changes.
- **SC-006**: Zero broken anchor links result from the common-workflows restructuring -- verified by checking external link patterns.

## Clarifications

### Session 2026-03-31

- Q: Should the Developer Guide's "Daily Workflow" section be updated to reflect the unleash-centered workflow? → A: Yes. The daily workflow should be focused on specify -> unleash (maybe back to clarify) -> finale, replacing the current generic session flow.

## Assumptions

- The blog post will follow the same format and quality standard as the existing Gaze blog posts ("Why Contract Coverage" and "Gaze in Practice") -- technical depth with real-world grounding, not marketing fluff.
- The blog post will describe the current `/unleash` behavior without hardcoding version numbers, so it remains accurate across minor releases.
- The homepage feature card update will modify the existing "Speckit Workflow" card text rather than adding a 5th card, to maintain the current 4-card grid layout.
- The common-workflows restructuring will reorder existing H2 sections without rewriting their content (content was just written in spec 012).
- The anchor slug `#new-feature-end-to-end` will be preserved using Hugo's heading ID override syntax or by keeping the exact heading text.
- The blog post's example will use a realistic but generic feature description (not a real project) to avoid coupling to specific repo state.
- The Getting Started landing page's Common Workflows link description will be updated in the list-item text, not requiring any structural page changes.
