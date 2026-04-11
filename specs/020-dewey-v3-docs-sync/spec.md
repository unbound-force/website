# Feature Specification: Dewey v3.0.0 Documentation Sync

**Feature Branch**: `020-dewey-v3-docs-sync`  
**Created**: 2026-04-11  
**Status**: Draft  
**Input**: User description: "issue #28 — sync dewey knowledge compilation, linting, and trust tiers (v3.0.0)"

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Accurate Tool Count and Capabilities (Priority: P1)

A visitor reads the Dewey project page, team page, or getting-started knowledge guide and sees the correct number of MCP tools (48), the correct number of categories, and the correct number of content source types (4). All three pages agree with each other.

**Why this priority**: Incorrect numbers directly undermine credibility. A visitor who cross-references pages and finds conflicting tool counts (40 vs 48) will question the accuracy of everything else on the site. This is the simplest fix with the highest trust impact.

**Independent Test**: Search the built site for all mentions of "MCP tools" and "categories" and verify every instance states the same correct numbers. Search for "source types" and verify all instances include the code source type.

**Acceptance Scenarios**:

1. **Given** a visitor on the Dewey project page, **When** they read the tool count, **Then** it states 48 MCP tools with the correct category count.
2. **Given** a visitor on the Dewey team page, **When** they read the tool count, **Then** it matches the project page exactly.
3. **Given** a visitor on the knowledge getting-started guide, **When** they read about Dewey's capabilities, **Then** the tool count matches the other two pages.
4. **Given** a visitor on any page that lists content source types, **When** they read the list, **Then** it includes all four types: local disk, GitHub API, web crawl, and code (Go AST parsing).

---

### User Story 2 — Knowledge Compilation, Linting, and Promotion Documentation (Priority: P1)

A visitor who wants to understand how Dewey manages knowledge quality can find documentation explaining the compile, lint, and promote capabilities — what they do, when to use them, and how they fit into the knowledge lifecycle.

**Why this priority**: These are the headline features of Dewey v3.0.0 and the reason the tool count jumped from 40 to 48. Omitting them makes the site materially incomplete for anyone evaluating Dewey's current capabilities.

**Independent Test**: A visitor with no prior Dewey knowledge can navigate from the Dewey project page to an explanation of compile, lint, and promote within two clicks.

**Acceptance Scenarios**:

1. **Given** a visitor on the Dewey project page, **When** they look for knowledge management capabilities, **Then** they find descriptions of compile, lint, and promote with enough context to understand their purpose.
2. **Given** a visitor on the knowledge getting-started guide, **When** they look for CLI commands, **Then** they find `dewey compile`, `dewey lint`, and `dewey promote` documented with usage descriptions.
3. **Given** a visitor on the Dewey team page, **When** they review the MCP tools table, **Then** the table includes the compile, lint, and promote tools.

---

### User Story 3 — Trust Tiers Documentation (Priority: P2)

A visitor who wants to understand how Dewey distinguishes between different quality levels of stored knowledge can find documentation explaining the trust tier system (authored, validated, draft) and how it affects search and retrieval.

**Why this priority**: Trust tiers are an architectural concept that affects how agents interact with Dewey's knowledge base. Without documentation, visitors cannot understand how Dewey ensures knowledge quality — a key differentiator from simple vector stores. This is P2 rather than P1 because trust tiers are a supporting concept that enriches the compile/lint/promote story rather than standing alone.

**Independent Test**: A visitor can find and understand the three trust tiers and how they influence search results without needing to read the Dewey source code.

**Acceptance Scenarios**:

1. **Given** a visitor on the knowledge getting-started guide, **When** they read about storing and retrieving knowledge, **Then** they find an explanation of the three trust tiers and their meaning.
2. **Given** a visitor reading about semantic search capabilities, **When** they look for filtering options, **Then** they learn that search results can be filtered by trust tier.

---

### User Story 4 — Updated store_learning API Documentation (Priority: P2)

A visitor who wants to understand how agents store learnings in Dewey can find accurate documentation of the `store_learning` tool, including the current parameter names (`tag` singular, required; `category` optional) and the identity format returned.

**Why this priority**: The `store_learning` tool is currently not documented with its parameters anywhere on the site (only mentioned conceptually in a blog post). The API changed in v3.0.0 (`tags` plural became `tag` singular, `category` was added). Visitors and agent authors need accurate parameter documentation. This is P2 because it serves a narrower audience (people configuring agents to use Dewey) than the broader capability documentation in P1.

**Independent Test**: A visitor can find the `store_learning` tool documented with its current parameters, understand what each parameter does, and know what format the response returns.

**Acceptance Scenarios**:

1. **Given** a visitor on the knowledge getting-started guide, **When** they look for how to store learnings, **Then** they find `store_learning` documented with its `tag` (required) and `category` (optional) parameters.
2. **Given** a visitor reading about the `store_learning` tool, **When** they review the response format, **Then** they learn that it returns a `{tag}-{sequence}` identity string.
3. **Given** a visitor reading about semantic search results, **When** they review what metadata is returned, **Then** they see that results include `created_at`, `category`, and `tier` fields.

---

### User Story 5 — Bug Fix Note for Web Crawler (Priority: P3)

A visitor reading about Dewey's web crawling capabilities learns that complex sites like pkg.go.dev are now indexed correctly. This absorbs the minor documentation note from issue #29.

**Why this priority**: This is a minor addition — a single sentence noting improved web crawler reliability. The existing documentation already describes web crawling as working correctly; these were code bugs, not doc bugs. Including it here avoids the overhead of a separate spec for a one-line change.

**Independent Test**: The web source section of the knowledge guide mentions that complex documentation sites (e.g., pkg.go.dev) are supported.

**Acceptance Scenarios**:

1. **Given** a visitor on the knowledge getting-started guide, **When** they read the web source configuration section, **Then** they find a note confirming that complex documentation sites like pkg.go.dev are supported.

---

### Edge Cases

- What happens when a visitor follows a link from the existing Karpathy blog post (which speculated about a future `dewey compile` command) to the docs? The docs should now confirm that compile exists, creating a coherent narrative.
- What happens if the upstream Dewey tool count changes again before this spec is implemented? The spec should document the source of truth (Dewey repository README or server.go) so future syncs follow the same pattern.
- What happens if a page mentions tool counts inline in prose vs in headings? Both must be updated — search for all occurrences, not just the most prominent ones.

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: The Dewey project page (`content/docs/projects/dewey.md`) MUST state the correct MCP tool count (48) and correct category count, consistent with the upstream Dewey repository.
- **FR-002**: The Dewey team page (`content/docs/team/dewey.md`) MUST state the same MCP tool count and category count as the project page.
- **FR-003**: The knowledge getting-started guide (`content/docs/getting-started/knowledge.md`) MUST state the same MCP tool count as the project page, where tool counts are mentioned.
- **FR-004**: All pages that list Dewey content source types MUST include all four types: local disk, GitHub API, web crawl, and code (Go AST parsing).
- **FR-005**: The Dewey project page MUST describe the compile, lint, and promote capabilities at a level appropriate for evaluators (what they do, why they matter).
- **FR-006**: The knowledge getting-started guide MUST document the `dewey compile`, `dewey lint`, and `dewey promote` CLI commands with usage descriptions.
- **FR-007**: The Dewey team page MUST include compile, lint, and promote in the MCP tools table.
- **FR-008**: The knowledge getting-started guide MUST explain the trust tier system (authored, validated, draft) and how tiers affect search and retrieval.
- **FR-009**: The knowledge getting-started guide MUST document the `store_learning` tool with its current parameters: `tag` (singular, required), `category` (optional, values: decision/pattern/gotcha/context/reference), and `information` (required).
- **FR-010**: The knowledge getting-started guide MUST document the `store_learning` response format (`{tag}-{sequence}` identity string).
- **FR-011**: The knowledge getting-started guide MUST document that `semantic_search` results include `created_at`, `category`, and `tier` metadata.
- **FR-012**: The knowledge getting-started guide MUST document that `semantic_search_filtered` accepts a `tier` parameter for trust-tier filtering.
- **FR-013**: The web source section of the knowledge getting-started guide MUST note that complex documentation sites (e.g., pkg.go.dev) are supported by the web crawler.
- **FR-014**: All content MUST be sourced from the upstream Dewey repository (README.md, AGENTS.md, source code, PR #42, spec 013). No capabilities may be fabricated or overstated.
- **FR-015**: The site MUST build successfully (`npm run build`) with no errors after all changes.
- **FR-016**: All new content MUST render correctly in both light and dark mode.

### Key Entities

- **Trust Tier**: A quality classification for stored knowledge. Three tiers exist: authored (human-written content), validated (machine-generated content reviewed and approved by a human), and draft (machine-generated content not yet reviewed). Tiers affect search result ranking and filtering.
- **Knowledge Compilation**: The process of clustering stored learnings by topic and synthesizing them into current-state articles via LLM. Raw learnings are an append-only event log; compiled articles are materialized views rebuilt from learnings.
- **Knowledge Linting**: Automated quality checks that detect stale decisions, uncompiled learnings, embedding gaps, and contradictions. Some checks are mechanically fixable; others require compilation.

## Scope & Constraints

### In Scope

- Updating tool counts, category counts, and source type counts on all three affected pages.
- Adding compile, lint, and promote capability descriptions to the project page.
- Adding compile, lint, and promote CLI command documentation to the knowledge guide.
- Adding compile, lint, and promote to the MCP tools table on the team page.
- Adding trust tier documentation to the knowledge guide.
- Updating store_learning API documentation on the knowledge guide.
- Adding a web crawler reliability note to the knowledge guide.

### Out of Scope

- Blog content (issue #30 covers a separate blog post and should be a separate spec).
- Changes to the homepage, navigation menus, or site structure.
- New pages — all changes are updates to existing pages.
- Upstream Dewey code changes or schema documentation.
- Documentation of the `llm/` package or Synthesizer interface (internal implementation detail).

## Dependencies & Assumptions

### Dependencies

- **Spec 010 (v0.5.0 docs sync)**: Established the "40 MCP tools across 10 categories" baseline that this spec updates. The pattern of searching for all tool count mentions and reconciling them was established there.
- **Spec 014 (dewey-docs-gaps)**: Added the knowledge guide web source section and the first Dewey blog post. This spec extends that same knowledge guide.
- **Upstream Dewey PR #42**: The source of truth for all v3.0.0 changes. Content must be derived from this PR and the associated spec 013-knowledge-compile.

### Assumptions

- The upstream Dewey README has been updated to reflect the 48-tool count. If not, use server.go tool registrations as the authoritative source.
- The existing page structure and heading hierarchy on all three affected pages is stable and does not need restructuring — only content additions and number updates.
- The Karpathy blog post (`dewey-vs-karpathy.md`) does not need updates as part of this spec. Its speculative mention of `dewey compile` will naturally become accurate once the docs confirm the feature exists.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: All mentions of Dewey MCP tool counts across the entire site state the same number (48), with zero inconsistencies.
- **SC-002**: A visitor with no prior Dewey knowledge can find and understand compile, lint, and promote within two clicks from the Dewey project page.
- **SC-003**: The knowledge getting-started guide documents all three new CLI commands (compile, lint, promote), the trust tier system, and the updated store_learning parameters.
- **SC-004**: The site builds successfully (`npm run build`) with zero errors and all pages render correctly in both light and dark mode.
- **SC-005**: All content on affected pages is consistent with the upstream Dewey repository — no fabricated or overstated capabilities.
