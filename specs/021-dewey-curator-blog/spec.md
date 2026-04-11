# Feature Specification: How Dewey Became a Knowledge Curator Blog Post

**Feature Branch**: `021-dewey-curator-blog`  
**Created**: 2026-04-11  
**Status**: Draft  
**Input**: User description: "issue #30 — blog: How Dewey Became a Knowledge Curator — From Passive Index to Active Synthesis"

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Narrative Journey from Comparison to Implementation (Priority: P1)

A reader who followed the earlier "The Librarian vs The Index" blog post (which compared Dewey and Karpathy's approaches and speculated about future features like `dewey compile`) reads this new post and learns how those speculative ideas became real, shipped features in Dewey v3.0.0. The post tells the engineering story: what problem was solved, what architectural insights emerged, and what the team learned building it.

**Why this priority**: This is the core purpose of the blog post. The predecessor post explicitly speculated about `dewey compile` and trust tiers as future possibilities. This post closes that narrative loop with "we built it, here's what happened." Without this narrative thread, the post is a generic feature announcement rather than a compelling sequel.

**Independent Test**: A reader who has read the Karpathy comparison post can follow this post as a natural continuation. A reader who has NOT read the Karpathy post can still understand the core story without feeling lost.

**Acceptance Scenarios**:

1. **Given** a reader familiar with the Karpathy comparison post, **When** they read this post, **Then** they recognize references to the earlier comparison and understand how the speculative ideas became features.
2. **Given** a reader with no prior Dewey knowledge, **When** they read this post, **Then** they understand the problem (learnings that never compound), the solution (compile, lint, promote), and the architectural insight (event sourcing) without needing to read the predecessor post.
3. **Given** a reader, **When** they finish the post, **Then** they have a clear understanding of what knowledge compilation does and why it matters for AI agent workflows.

---

### User Story 2 — Architectural Insights for Technical Readers (Priority: P1)

A technical reader (developer, architect, or AI tooling enthusiast) reads the post and gains insight into two architectural patterns: the event sourcing model for knowledge management (raw learnings as append-only events, compiled articles as materialized views) and the dual-mode LLM pattern (agent-as-LLM for MCP tool calls, local Ollama for CLI compilation).

**Why this priority**: Architectural insights are what make a blog post shareable and referenceable. Feature announcements are quickly forgotten; novel patterns persist. These two insights are genuinely novel and differentiate this post from a changelog.

**Independent Test**: A technical reader can identify and articulate at least two architectural patterns from the post that they could apply to their own projects.

**Acceptance Scenarios**:

1. **Given** a technical reader, **When** they read the event sourcing section, **Then** they understand the distinction between raw learnings (events) and compiled articles (materialized views) and why this model was chosen.
2. **Given** a technical reader, **When** they read the dual-mode LLM section, **Then** they understand why a local LLM is used for CLI compilation while MCP tool calls use the agent's own LLM.
3. **Given** a technical reader, **When** they read the knowledge linting section, **Then** they understand the four quality checks (stale decisions, uncompiled learnings, embedding gaps, contradictions) and the mechanical vs semantic fix distinction.

---

### User Story 3 — Links to Updated Documentation (Priority: P2)

A reader who wants to try the features described in the post can follow links to the updated Dewey documentation pages (which were synced in spec 020). The post references docs for CLI details rather than embedding reference material that will drift.

**Why this priority**: The blog post should drive readers to the authoritative reference docs rather than duplicating them. This reduces maintenance burden (Minimal Footprint principle) and keeps the post focused on narrative rather than syntax.

**Independent Test**: Every internal link in the post resolves to a valid page. CLI command syntax mentioned in the post matches what the linked docs describe.

**Acceptance Scenarios**:

1. **Given** a reader who wants to try `dewey compile`, **When** they click a link in the post, **Then** they arrive at the knowledge getting-started guide's Knowledge Lifecycle section with full CLI usage details.
2. **Given** a reader interested in trust tiers, **When** they click a link, **Then** they arrive at the Trust Tiers documentation on the knowledge guide.
3. **Given** a reader who reads the post in full, **When** they count internal links, **Then** they find links to at least the Dewey project page and the knowledge getting-started guide.

---

### Edge Cases

- The post must credit Karpathy fairly — the original comparison post took a balanced approach and this sequel must maintain that tone. The framing is "we learned from the comparison" not "we won."
- The post must avoid time-sensitive language ("just shipped," "this week") to remain relevant over time. The date in frontmatter provides temporal context.
- The post must not duplicate CLI syntax or API parameter details that live in the docs pages updated by spec 020. Reference docs by linking, not by inlining.
- The post must not overclaim the maturity of knowledge compilation — it is a new feature shipped in v3.0.0. Describe what it does accurately without implying production-hardened reliability across all environments.
- If the Karpathy comparison post's URL changes, internal links use site-relative paths (`/blog/dewey-vs-karpathy/`) so they remain valid.

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: The blog post MUST be a single Markdown file at `content/blog/dewey-curator-blog.md` with valid Hugo/Doks frontmatter (title, description, lead, slug, date, draft, weight, toc, categories, tags, contributors).
- **FR-002**: The post MUST follow the five-section outline from issue #30: (1) The Problem: Learnings That Never Compound, (2) From Karpathy to Code: Three Key Ideas, (3) The Event Sourcing Insight, (4) Knowledge Linting: The Self-Healing Index, (5) What We Learned Building It.
- **FR-003**: The post MUST reference the predecessor blog post ("The Librarian vs The Index") by linking to it, establishing the narrative continuity between comparison and implementation.
- **FR-004**: The post MUST explain the event sourcing architecture: raw learnings as an append-only event log, compiled articles as materialized views rebuilt from learnings.
- **FR-005**: The post MUST explain the knowledge linting system with its four quality checks (stale decisions, uncompiled learnings, embedding gaps, contradictions) and distinguish between mechanical fixes and semantic fixes.
- **FR-006**: The post MUST explain trust tiers (authored/validated/draft) and how they distinguish human-reviewed content from machine-generated content.
- **FR-007**: The post MUST explain the dual-mode LLM pattern: agent LLM for MCP tool calls, local Ollama for CLI compilation.
- **FR-008**: The post MUST link to the updated Dewey documentation pages (updated in spec 020) for CLI syntax and API reference details rather than duplicating that content.
- **FR-009**: The post MUST NOT contain implementation details that will drift (specific CLI flag syntax, parameter names, response formats). These details live in the docs. The post describes concepts and architecture.
- **FR-010**: The post MUST NOT use time-sensitive language ("just shipped," "this week," "recently"). The frontmatter date provides temporal context.
- **FR-011**: The post MUST credit Karpathy's approach fairly and acknowledge where it excels (small curated datasets, personal research). The tone is "inspired by" not "better than."
- **FR-012**: The post MUST source all factual claims from upstream Dewey PR #42 (spec 013-knowledge-compile), the Dewey repository, and the predecessor blog post. No capabilities may be fabricated or overstated.
- **FR-013**: The post MUST include the "What We Learned Building It" section with at least two genuine engineering insights from the v3.0.0 development (e.g., category-aware resolution in prompts, 124 tests across 7 packages).
- **FR-014**: The site MUST build successfully (`npm run build`) with no errors after adding the new blog post.
- **FR-015**: The post MUST render correctly in both light and dark mode with the Doks theme.

## Scope & Constraints

### In Scope

- One new blog post file at `content/blog/dewey-curator-blog.md`.
- The five-section narrative structure from issue #30.
- Links to existing documentation pages (updated by spec 020).

### Out of Scope

- Changes to existing blog posts (the Karpathy comparison post is not modified).
- Changes to documentation pages (spec 020 already handled this).
- Changes to homepage, navigation, or site structure.
- Social media assets, press releases, or announcement drafts.
- Updates to the blog post's stale "40 tools" reference in `dewey-vs-karpathy.md` (that is a separate consideration, not part of this spec).

## Dependencies & Assumptions

### Dependencies

- **Spec 020 (dewey-v3-docs-sync)**: The documentation pages linked from this post were updated by spec 020. Spec 020 is now complete and merged. The blog post links to the current-state docs, not to stale content.
- **Spec 019 (dewey-karpathy-blog)**: Created the predecessor blog post ("The Librarian vs The Index"). This new post is a narrative sequel. Spec 019 is complete.
- **Upstream Dewey PR #42**: The source of truth for all v3.0.0 feature details. Content must be derived from this PR and the associated spec 013-knowledge-compile.

### Assumptions

- The blog section already exists on the site (5 published posts as of spec 019). No navigation or section setup is needed.
- The post will use the same frontmatter format as existing blog posts (weight, categories, tags, contributors).
- The post targets a technical audience familiar with AI coding agents but not necessarily with Dewey specifically. It should be accessible without prior Dewey knowledge.
- The post will be approximately 1200-1800 words — long enough for depth, short enough to read in one sitting. This matches the length of the predecessor Karpathy comparison post.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: A reader who has read the Karpathy comparison post recognizes this as a natural sequel and can follow the narrative thread from comparison to implementation.
- **SC-002**: A reader with no prior Dewey knowledge can understand the problem (learnings that never compound), the solution (compile/lint/promote), and at least one architectural insight (event sourcing or dual-mode LLM) after reading the post.
- **SC-003**: The post contains zero duplicated reference material — all CLI syntax, API parameters, and configuration details are linked to docs, not inlined.
- **SC-004**: The site builds successfully (`npm run build`) with zero errors and the post renders correctly in both light and dark mode.
- **SC-005**: All internal links in the post resolve to valid pages. All factual claims are traceable to upstream source material (PR #42, spec 013, predecessor blog post).
