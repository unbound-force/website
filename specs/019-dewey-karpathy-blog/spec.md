# Feature Specification: Dewey vs Karpathy Blog Post

**Feature Branch**: `019-dewey-karpathy-blog`
**Created**: 2026-04-07
**Status**: Draft
**Input**: Issue #26 — Blog post comparing Dewey and Karpathy's "LLM Knowledge Base" approach to AI agent knowledge management.

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Blog Post: Architectural Comparison (Priority: P1)

A developer using AI coding tools discovers a blog post that compares two approaches to the "context loss on restart" problem: Karpathy's "LLM Knowledge Base" (the LLM compiles and maintains a markdown wiki) and Dewey (indexes existing repos with semantic search via MCP). The post helps the reader understand when each approach is better, where they complement each other, and how to choose. The reader leaves with a clear mental model of both approaches and a concrete next step.

**Why this priority**: Karpathy's X post (April 3, 2026) is actively being discussed. The VentureBeat article is 4 days old. Publishing a thoughtful comparison while the topic is current maximizes social sharing and SEO value. The comparison positions Dewey in a conversation that's already happening rather than starting a new one.

**Independent Test**: Navigate to `/blog/` and verify the post exists, renders correctly, covers both approaches fairly, includes a comparison table, and links to the Getting Started page.

**Acceptance Scenarios**:

1. **Given** a visitor on the blog index, **When** they browse articles, **Then** they see a post about knowledge management approaches with a title that frames the comparison (not a product pitch).
2. **Given** a reader of the blog post, **When** they read the problem statement, **Then** they recognize the "context loss across sessions" problem from their own AI coding experience.
3. **Given** a reader of the blog post, **When** they read the Karpathy section, **Then** they understand the compile-then-navigate approach: LLM reads raw materials, writes structured wiki with backlinks, runs self-healing linting passes, navigates via summaries and indexes.
4. **Given** a reader of the blog post, **When** they read the Dewey section, **Then** they understand the index-then-search approach: indexes existing repos with embeddings, multi-repo semantic search via MCP, 4 source types (disk, GitHub, web, code), no LLM involvement in the indexing step.
5. **Given** a reader of the blog post, **When** they look at the comparison table, **Then** they find a side-by-side comparison covering at least: core metaphor, who organizes, search method, embeddings, scale, multi-repo support, self-healing, protocol, multi-agent support, local-only.
6. **Given** a reader of the blog post, **When** they read the "where they complement" section, **Then** they understand that the compiled wiki becomes a Dewey disk source, and that the ideal system could use both.
7. **Given** a reader of the blog post, **When** they look for how to get started, **Then** they find install instructions and a link to the Quick Start page.

---

### Edge Cases

- The post must credit Karpathy fairly — the synthesis model is better for small, curated datasets and personal research. The post must not position Dewey as "better" in all cases.
- The post must avoid time-sensitive language ("just announced," "this week") to remain relevant as the discussion fades. Use the date in frontmatter, not in prose.
- The post must not overclaim Dewey's capabilities — the "code source" indexing (Go AST parsing) is new (Dewey PR #32) and should be described accurately.
- If the VentureBeat article URL changes or goes behind a paywall, the post should reference Karpathy's X post as the primary source.

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: The blog post MUST describe the shared problem (context loss across AI sessions, cross-repo knowledge fragmentation) before introducing either solution.
- **FR-002**: The blog post MUST describe Karpathy's approach (LLM-as-librarian, raw materials -> compiled wiki, linting passes, backlinks, no embeddings) accurately and fairly, citing his X post and the VentureBeat article.
- **FR-003**: The blog post MUST describe Dewey's approach (index existing repos, local embeddings, 4 source types, MCP protocol, multi-repo) accurately, with the current tool count and source types verified against the Dewey README.
- **FR-004**: The blog post MUST include a comparison table covering at least 10 dimensions (core metaphor, who organizes, search method, embeddings, scale target, multi-repo, self-healing, protocol, multi-agent, local-only).
- **FR-005**: The blog post MUST include a "where they complement each other" section explaining how the compiled wiki can become a Dewey disk source.
- **FR-006**: The blog post MUST include a decision guide ("which should you use?") with clear criteria for each approach and a "use both" option.
- **FR-007**: The blog post MUST include a Getting Started section with install instructions and a link to the Quick Start page and knowledge retrieval guide.
- **FR-008**: The blog post MUST follow existing blog conventions (YAML frontmatter with title, description, lead, slug, date, categories, tags, contributors; body starting with H2; no time-sensitive language in prose).
- **FR-009**: The blog post MUST NOT position Dewey as universally superior — it must honestly acknowledge where Karpathy's approach is better (active synthesis, self-healing, curated small datasets).
- **FR-010**: All changes MUST pass `npm run build` without errors.

## Success Criteria _(mandatory)_

- **SC-001**: The blog post exists at a discoverable URL and its frontmatter description communicates the comparison angle (not a product pitch).
- **SC-002**: A reader can understand both approaches without prior knowledge of either Dewey or Karpathy's system.
- **SC-003**: The comparison table has at least 10 dimensions.
- **SC-004**: The post credits Karpathy's X post and links to the VentureBeat article.
- **SC-005**: `npm run build` succeeds with zero errors.

## Assumptions

- The blog post will follow the same format and quality standard as the existing blog posts — technical depth with real-world grounding.
- The title will lead with the comparison framing (not a product name) to attract readers interested in the problem, not just Dewey users.
- The Dewey README's current MCP tool count is the authoritative source for capability claims. The issue's "44 MCP tools" claim will be verified and corrected if needed.
- Dewey now has 4 source types: disk, GitHub, web, and code (Go AST parsing, added in PR #32). This is a significant capability difference from what the issue assumed.
- The post will reference Karpathy's X post as the primary source and the VentureBeat article as the explanatory context.
- No architecture diagrams — the post will use text-based comparison tables and code examples. Diagrams can be added later if created manually.
- The `store_learning` tool is Dewey's primitive version of the "compilation step" and should be mentioned in the "where they complement" section.
