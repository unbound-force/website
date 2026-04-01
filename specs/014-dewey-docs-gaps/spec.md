# Feature Specification: Dewey Documentation Gaps

**Feature Branch**: `014-dewey-docs-gaps`
**Created**: 2026-04-01
**Status**: Draft
**Input**: User description: "Address Dewey documentation gaps from three perspectives (tech writer, PR officer, prospective developer): document uf init auto-generated sources.yaml, add toolstack web source guidance, and create a blog post on Dewey knowledge retrieval."

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Document What `uf init` Auto-Generates for Dewey Sources (Priority: P1)

A new developer has just run `uf setup` and wants to understand what Dewey was configured with. The `knowledge.md` page tells them to `$EDITOR .dewey/sources.yaml` but never explains what `uf init` already put in that file. The developer opens `sources.yaml` and sees entries for sibling repos, an org-level workspace source, and a GitHub API source — but has no documentation explaining why those entries exist, how they were detected, or what they do.

**Why this priority**: This is the highest-impact gap because every new user encounters it. `uf init` does sophisticated auto-detection (scanning `../` for sibling repos with `.git/`, extracting the GitHub org from the git remote) but the result is unexplained to the user. Without understanding their starting point, users cannot effectively customize their configuration. The information exists in PR #63 and the design doc but has never been surfaced on the website.

**Independent Test**: Read the "Configure Content Sources" section of `knowledge.md` and verify that it explains what `uf init` auto-generates before showing the manual YAML examples.

**Acceptance Scenarios**:

1. **Given** a developer reading the source configuration section, **When** they look for what `uf init` created, **Then** they find a subsection explaining: sibling repo detection (`../` scan for `.git/` directories), GitHub org extraction from git remote, per-repo disk sources, org-level disk source, and GitHub API source with auto-detected repo list.
2. **Given** a developer reading the source configuration section, **When** they look at the auto-generated config example, **Then** they see a realistic multi-repo `sources.yaml` with comments explaining each source group.
3. **Given** a developer reading the source configuration section, **When** they look for the ownership model, **Then** they find that `sources.yaml` is user-owned after creation — `uf init` never overwrites a customized config (detected by counting `- id:` entries).
4. **Given** a developer with no sibling repos or no GitHub remote, **When** they read the auto-detection explanation, **Then** they find that `uf init` gracefully degrades — fewer sources are generated, and the config is still valid.

---

### User Story 2 — Add Toolstack Web Source Guidance (Priority: P1)

A developer has Dewey running with disk and GitHub sources (auto-configured by `uf init`) but their agents are generating code using stale or incorrect API patterns because they lack access to current framework and library documentation. The website never proactively suggests that the developer should add web sources for their project's toolstack — even though this is the single most impactful customization for improving agent context quality.

**Why this priority**: This is equal priority to US1 because it directly affects the quality of AI agent output. A developer who adds web sources for their frameworks (Go stdlib, Cobra, GORM, React, Next.js, etc.) immediately gets better code generation because agents can reference current API docs instead of relying on training data. The current documentation shows the web source YAML syntax but gives no guidance on _what to index_ or _why it matters_.

**Independent Test**: Read `knowledge.md` and verify there is a "Recommended Configuration" or "Extending Your Sources" section that explains the toolstack web source strategy with concrete examples.

**Acceptance Scenarios**:

1. **Given** a developer who has Dewey running with the default config, **When** they read about extending sources, **Then** they find proactive guidance saying: "Add web sources for your project's key frameworks and libraries so agents can reference current API documentation."
2. **Given** a Go developer reading the recommended sources section, **When** they look for a template, **Then** they find a concrete example with sources for `pkg.go.dev/std`, `cobra.dev`, and at least one database/ORM framework.
3. **Given** a TypeScript developer reading the recommended sources section, **When** they look for a template, **Then** they find a concrete example with sources for the TypeScript handbook, their framework docs (React, Next.js, etc.), and relevant library docs.
4. **Given** a developer reading the recommended sources section, **When** they look for why this matters, **Then** they find an explanation of the benefit: agents reference current API docs instead of training data, reducing hallucinated API calls and outdated patterns.

---

### User Story 3 — Blog Post: "Your AI Agent Doesn't Read the Docs" (Priority: P2)

A developer using any AI coding tool (not just Unbound Force) discovers a blog post about a universal problem: AI agents generate code using training data that may be months old, when current API docs are available online. The post shows how Dewey bridges this gap by indexing toolstack documentation and making it available through semantic search. The developer understands the concept, sees a before/after example, and has a clear path to try it.

**Why this priority**: The blog post serves a different audience than the docs updates (US1/US2). It addresses an external, SEO-discoverable audience — developers who haven't heard of Dewey but experience the "stale training data" problem daily. The existing Gaze blog posts drove organic discovery; a Dewey blog post would do the same for the knowledge retrieval capability. Ranked P2 because the docs fixes (US1/US2) serve existing users who need this information now, while the blog post builds future pipeline.

**Independent Test**: Navigate to `/blog/` and verify the post exists, covers the stale-training-data problem, shows how Dewey web sources solve it, and links to the Quick Start and `knowledge.md` pages.

**Acceptance Scenarios**:

1. **Given** a developer on the blog index, **When** they browse articles, **Then** they see a post about AI agents and documentation access with a title that communicates the problem, not the tool.
2. **Given** a reader of the blog post, **When** they read the problem statement, **Then** they recognize the "stale API patterns from training data" problem from their own experience.
3. **Given** a reader of the blog post, **When** they read the solution section, **Then** they understand how adding web sources to `sources.yaml` gives agents access to current API documentation via semantic search.
4. **Given** a reader of the blog post, **When** they look for how to get started, **Then** they find install instructions and a link to the Quick Start page and the `knowledge.md` source configuration section.

---

### User Story 4 — Cross-Reference Dewey Source Configuration from Developer Guide (Priority: P3)

A developer reading the `uf init` section of the Developer Guide sees that it creates `.dewey/sources.yaml` but has no link to where they can learn about customizing it. After this change, the `uf init` documentation links to the `knowledge.md` source configuration section with a brief note about extending the auto-generated config with web sources.

**Why this priority**: This is a discovery path fix — if a developer is reading about `uf init` (not about Dewey specifically), they should still find their way to the source configuration guidance. Ranked P3 because US1 and US2 create the content; US4 creates a cross-reference to it.

**Independent Test**: Read the `uf init` section in `developer.md` and verify it mentions `sources.yaml` customization and links to `knowledge.md`.

**Acceptance Scenarios**:

1. **Given** a developer reading the `uf init` documentation, **When** they see the Dewey initialization mention, **Then** they find a note explaining that `uf init` auto-detects sibling repos and GitHub org for the source config, with a link to the knowledge page for customization guidance.

---

### Edge Cases

- What happens when a developer runs `dewey init` directly (without `uf init`)? They get the bare default (single disk source). The documentation must distinguish between the `dewey init` default and the `uf init` enhanced config.
- What happens when a developer has no sibling repos? `uf init` generates a config with only the current repo disk source and the GitHub API source (if a GitHub remote exists). The documentation must note this as a valid scenario.
- What happens when a developer adds web sources but the URLs change? The documentation should advise using stable documentation URLs (e.g., `pkg.go.dev/std` not `go.dev/doc/effective_go`) and checking `dewey doctor` for fetch errors.
- What happens when a user reads the blog post but doesn't use the Unbound Force swarm? The blog post should explain the concept generically (semantic search over API docs) while showing Dewey as the implementation. The concept is valuable beyond the specific tool.

## Requirements _(mandatory)_

### Functional Requirements

**`uf init` Auto-Generated Config (US1):**

- **FR-001**: The `knowledge.md` page MUST include a subsection before the individual source type examples explaining what `uf init` auto-generates: sibling repo detection, GitHub org extraction, and the resulting multi-source config.
- **FR-002**: The subsection MUST include an example of the auto-generated `sources.yaml` showing: per-repo disk sources for detected siblings, an org-level disk source, and a GitHub API source with auto-detected repos.
- **FR-003**: The subsection MUST explain the ownership model: `sources.yaml` is user-owned after creation, `uf init` detects customization (>1 source entry) and skips regeneration.
- **FR-004**: The subsection MUST distinguish between `dewey init` (bare single-source default) and `uf init` (enhanced multi-repo config generated between `dewey init` and `dewey index`).

**Toolstack Web Source Guidance (US2):**

- **FR-005**: The `knowledge.md` page MUST include a "Recommended Configuration" or "Extending Your Sources" section after the source type reference that proactively guides users to add web sources for their toolstack.
- **FR-006**: The section MUST explain the benefit: agents reference current API documentation instead of training data, reducing hallucinated API calls and outdated patterns.
- **FR-007**: The section MUST include at least two language-specific templates — one for Go projects (pkg.go.dev, framework docs) and one for TypeScript/JavaScript projects (TypeScript handbook, framework docs).
- **FR-008**: The section MUST include guidance on choosing URLs: prefer stable documentation roots, use `depth` to control crawl scope, use `refresh_interval` for update frequency.

**Blog Post (US3):**

- **FR-009**: The website MUST have a blog post that describes the stale-training-data problem and how semantic search over toolstack documentation solves it.
- **FR-010**: The blog post MUST show a concrete before/after: what happens when an agent lacks API docs vs. when Dewey provides them through web sources.
- **FR-011**: The blog post MUST explain the three source types (disk, GitHub, web) at a conceptual level and why web sources are the most impactful customization.
- **FR-012**: The blog post MUST include a Getting Started section with a link to the Quick Start page and the `knowledge.md` source configuration section.
- **FR-013**: The blog post MUST follow existing blog conventions (YAML frontmatter with title, description, lead, slug, date, categories, tags, contributors; body starting with H2).

**Cross-Reference (US4):**

- **FR-014**: The `uf init` section in `developer.md` MUST mention `sources.yaml` auto-detection and link to the `knowledge.md` source configuration section for customization guidance.

**Cross-cutting:**

- **FR-015**: All documentation changes MUST pass `npm run build` without errors.
- **FR-016**: All cross-links MUST resolve to existing pages and anchors.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: A developer reading `knowledge.md` can identify exactly what `uf init` configured for their Dewey sources without opening `.dewey/sources.yaml` directly — the auto-generated config is fully explained on the page.
- **SC-002**: A developer reading `knowledge.md` finds proactive guidance to add web sources for their toolstack, with at least 2 language-specific templates they can copy-paste.
- **SC-003**: The blog post exists at a discoverable URL and its frontmatter description communicates the stale-training-data problem (not just "Dewey features") for search and social sharing relevance.
- **SC-004**: The `uf init` section in `developer.md` links to the source configuration section of `knowledge.md` — zero dead ends when reading about Dewey initialization.
- **SC-005**: `npm run build` succeeds with zero errors after all changes.

## Assumptions

- The `uf init` auto-detection behavior documented in PR #63 and the design doc (`openspec/changes/archive/2026-03-28-dewey-auto-sources/design.md`) is the authoritative source for how sources are generated.
- The blog post will follow the same format and quality standard as the existing blog posts — technical depth with real-world grounding, not a feature catalog.
- The blog post title should lead with the problem ("Your AI Agent Doesn't Read the Docs") not the tool ("Dewey Knowledge Retrieval Features"), following the pattern established by the Gaze blog posts.
- The Go and TypeScript web source templates are the two most useful because they match the project types that `uf init` detects via `go.mod` and `tsconfig.json/package.json`.
- The "Recommended Configuration" section will be placed after the three source type reference sections in `knowledge.md`, serving as a "now that you know the syntax, here's what to do with it" bridge.
- The `developer.md` cross-reference will be a 1-2 sentence addition to the existing `uf init` section, not a new section.
