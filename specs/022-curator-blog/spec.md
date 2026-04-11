# Feature Specification: Introducing The Curator Blog Post

**Feature Branch**: `022-curator-blog`  
**Created**: 2026-04-11  
**Status**: Draft  
**Input**: User description: "issue #36 — blog: Introducing The Curator — automated documentation governance for AI agent teams"

## User Scenarios & Testing _(mandatory)_

### User Story 1 — The Curator's Role and Why It Exists (Priority: P1)

A reader learns what The Curator is, why documentation drift is a persistent problem in AI agent workflows, and how The Curator addresses it. The post explains the gap: agents can ship features without documentation updates because no one checks. The Curator is a Divisor persona that runs during every code review, detects documentation gaps, and auto-files GitHub issues to track them.

**Why this priority**: This is the core premise of the blog post. Without explaining what The Curator does and why it matters, the rest of the post has no foundation. The problem (documentation drift in AI agent teams) is relatable to anyone using AI coding agents.

**Independent Test**: A reader with no prior knowledge of the Unbound Force swarm can understand the documentation drift problem and the Curator's solution after reading this section. The post is self-contained — it does not require reading prior blog posts.

**Acceptance Scenarios**:

1. **Given** a reader unfamiliar with Unbound Force, **When** they read the post, **Then** they understand the documentation drift problem and how The Curator addresses it.
2. **Given** a reader familiar with AI coding agents, **When** they read the post, **Then** they recognize the documentation drift pattern from their own experience.
3. **Given** a reader, **When** they finish reading, **Then** they understand that The Curator triages documentation gaps (identifies and files issues) but does not create the documentation itself — that separation of concerns is deliberate.

---

### User Story 2 — Technical Design Insights (Priority: P1)

A technical reader learns three interesting design decisions from building The Curator: (1) it is the first Divisor agent with bash access, restricted to `gh` CLI only, (2) the bootstrap paradox — The Curator cannot review its own creation because it does not exist yet during its own PR, and (3) the separation between triage (Curator) and creation (Scribe/Herald/Envoy).

**Why this priority**: Design insights are what make technical blog posts shareable. The bootstrap paradox is a genuinely interesting problem that developers encounter in self-referential systems. These insights elevate the post beyond a feature announcement.

**Independent Test**: A technical reader can identify and articulate at least two design patterns from the post (bash restriction, bootstrap paradox, or triage/creation separation) that they could apply to their own agent architectures.

**Acceptance Scenarios**:

1. **Given** a technical reader, **When** they read about bash access, **Then** they understand why The Curator needs shell access (to run `gh issue create`) and why it is restricted to only `gh` CLI commands.
2. **Given** a technical reader, **When** they read about the bootstrap paradox, **Then** they understand the self-referential problem: The Curator cannot review the PR that creates it, because it does not exist until that PR merges.
3. **Given** a technical reader, **When** they read about triage vs creation, **Then** they understand why The Curator files issues rather than creating documentation directly — and how Scribe, Herald, and Envoy handle the creation side.

---

### User Story 3 — Links to Documentation and Context (Priority: P2)

A reader who wants to learn more about The Divisor council, the existing personas, or how to configure The Curator can follow links to the relevant documentation pages on the Unbound Force website.

**Why this priority**: The blog post should drive readers to authoritative reference material rather than duplicating it. This is the same pattern established in spec 021 (Dewey curator blog) — narrative in the post, reference in the docs.

**Independent Test**: Every internal link in the post resolves to a valid page. The post links to at least the Divisor team page and the getting-started guide.

**Acceptance Scenarios**:

1. **Given** a reader who wants to understand the full Divisor council, **When** they click a link, **Then** they arrive at the Divisor team page with all persona descriptions.
2. **Given** a reader who wants to configure The Curator, **When** they look for setup instructions, **Then** the post links to the relevant docs or notes that The Curator is auto-discovered by the review council.

---

### Edge Cases

- The post must not overclaim The Curator's intelligence — it uses heuristic checks (file paths in the diff vs documentation pages), not semantic understanding of whether documentation is actually needed. Be honest about the limitations.
- The post must handle the persona count carefully — the existing team page says 8 personas (5 review + 3 content creation). The Curator is a 9th persona. The post should state the updated count clearly.
- The post must not duplicate agent configuration syntax or `gh` CLI commands — link to docs instead.
- The post must avoid time-sensitive language ("just shipped," "this week") — the frontmatter date provides temporal context.
- The bootstrap paradox story must be technically accurate — describe what actually happened during the PR, not a hypothetical scenario.

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: The blog post MUST be a single Markdown file at `content/blog/curator-blog.md` with valid Hugo/Doks frontmatter (title, description, lead, slug, date, draft, weight, toc, categories, tags, contributors).
- **FR-002**: The post MUST follow the three-section structure from issue #36: (1) The Problem — documentation drift, (2) The Solution — a dedicated triage agent, (3) The Interesting Bits — bash access, bootstrap paradox, triage vs creation separation.
- **FR-003**: The post MUST explain the documentation drift problem: agents ship features without documentation updates, and without a dedicated checker, these gaps accumulate silently.
- **FR-004**: The post MUST explain The Curator's role: it runs during every code review, checks for documentation gaps, and auto-files GitHub issues to track them.
- **FR-005**: The post MUST explain the triage vs creation separation: The Curator identifies gaps and files issues, while The Scribe (tech docs), The Herald (blog/announcements), and The Envoy (communications) handle content creation.
- **FR-006**: The post MUST describe The Curator's restricted bash access — the first Divisor agent with shell access, limited to `gh` CLI commands only.
- **FR-007**: The post MUST describe the bootstrap paradox: The Curator cannot review the PR that creates it, because it does not exist yet during its own review. Describe how this was handled in practice.
- **FR-008**: The post MUST link to the Divisor team page and at least one other internal documentation page for deeper context.
- **FR-009**: The post MUST NOT contain implementation details that will drift (agent prompt contents, specific heuristic rules, `gh` CLI syntax). These details live in the agent definition file and docs.
- **FR-010**: The post MUST NOT use time-sensitive language ("just shipped," "this week," "recently"). The frontmatter date provides temporal context.
- **FR-011**: The post MUST accurately state that The Curator is the 9th Divisor persona (or whatever the correct count is at implementation time), joining the existing review and content creation personas.
- **FR-012**: The post MUST source all factual claims from upstream PR #89 (unbound-force/unbound-force) and spec 026-documentation-curation. No capabilities may be fabricated or overstated.
- **FR-013**: The post MUST NOT overclaim The Curator's capabilities — acknowledge that it uses heuristic file-path matching, not semantic understanding, to detect documentation gaps.
- **FR-014**: The site MUST build successfully (`npm run build`) with no errors after adding the new blog post.
- **FR-015**: The post MUST render correctly in both light and dark mode with the Doks theme.

## Scope & Constraints

### In Scope

- One new blog post file at `content/blog/curator-blog.md`.
- The three-section narrative structure from issue #36.
- Links to existing documentation pages (Divisor team page, getting-started guides).

### Out of Scope

- Changes to existing blog posts or documentation pages.
- Updates to the Divisor team page to add The Curator persona (that should be a separate spec if the team page is not already updated).
- Changes to the homepage, navigation, or site structure.
- The Curator's agent definition file (`.opencode/agents/divisor-curator.md`) — that lives in the upstream `unbound-force/unbound-force` repo.
- Documentation of The Curator's heuristic rules or prompt contents.

## Dependencies & Assumptions

### Dependencies

- **Spec 021 (dewey-curator-blog)**: Established the pattern of narrative blog posts that link to docs instead of duplicating reference material. This spec follows the same pattern.
- **Spec 019 (dewey-karpathy-blog)**: Established the blog post frontmatter format and style conventions.
- **Upstream PR #89 (unbound-force/unbound-force)**: The source of truth for The Curator's implementation. Content must be derived from this PR and spec 026-documentation-curation.
- **Divisor Architecture (Spec 005, upstream)**: Defines the Divisor framework and persona architecture that The Curator extends.

### Assumptions

- The blog section already exists on the site (6 published posts as of spec 021). No navigation or section setup is needed.
- The post will use the same frontmatter format as existing blog posts (weight, categories, tags, contributors).
- The Divisor team page already lists the 8 existing personas. The blog post will reference the updated count (9) but does not modify the team page itself. This means a reader who follows the blog post's link to the team page will temporarily see a stale count until a separate spec updates the team page.
- The post targets a technical audience familiar with AI coding agents and code review automation, but not necessarily with the Unbound Force swarm. It should be accessible without prior Unbound Force knowledge.
- The post will be approximately 1200-1800 words, matching the length of previous blog posts (dewey-vs-karpathy, dewey-curator).
- The upstream PR #89 and spec 026 are the authoritative sources for all factual claims about The Curator's behavior and design.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: A reader with no prior Unbound Force knowledge can understand the documentation drift problem and The Curator's role after reading the post.
- **SC-002**: A technical reader can identify at least two design insights (bash restriction, bootstrap paradox, or triage/creation separation) from the post.
- **SC-003**: The post contains zero duplicated reference material — all agent configuration, CLI syntax, and heuristic rules are linked to docs, not inlined.
- **SC-004**: The site builds successfully (`npm run build`) with zero errors and the post renders correctly in both light and dark mode.
- **SC-005**: All internal links in the post resolve to valid pages. All factual claims are traceable to upstream source material (PR #89, spec 026).
