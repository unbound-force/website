# Feature Specification: v0.10.0 / v0.11.0 Documentation Sync

**Feature Branch**: `023-docs-v10-v11-sync`  
**Created**: 2026-04-11  
**Status**: Draft  
**Input**: User description: "issues #33, #34, #35 — document gatekeeping value protection rules, ensureGitignore behavior in uf init, and The Curator Divisor agent across website docs pages"

## User Scenarios & Testing _(mandatory)_

### User Story 1 — The Curator Documented on The Divisor Team Page (Priority: P1)

A visitor reading The Divisor team page learns about all nine Divisor personas, including The Curator — the documentation triage agent that detects documentation gaps during code review and auto-files GitHub issues. The persona count is updated from "eight" to "nine" across all pages that reference it. The Curator's five audit checklist items, its unique bash access (restricted to `gh` CLI), and its ownership boundaries are documented.

**Why this priority**: The blog post (spec 022, now published) references "nine personas" and links to the Divisor team page. A visitor who follows that link currently sees a page that says "eight personas" — a direct contradiction. Resolving this inconsistency is the highest-priority fix because it damages credibility for any reader of the blog post.

**Independent Test**: Search the entire `content/` directory for the word "eight" in the context of Divisor/personas/sub-personas. Zero matches should remain after this change. The Divisor team page lists nine personas with The Curator documented alongside the existing eight.

**Acceptance Scenarios**:

1. **Given** a visitor on the Divisor team page, **When** they read the persona list, **Then** they find nine personas including The Curator with its documentation triage role.
2. **Given** a visitor on the team index page (`_index.md`), **When** they read the Divisor summary, **Then** it references nine sub-personas, not eight.
3. **Given** a visitor on the contributing page, **When** they read the Divisor description, **Then** it references nine sub-personas, not eight.
4. **Given** a visitor on the common workflows page, **When** they read the code review persona table, **Then** The Curator appears with its documentation governance focus area.

---

### User Story 2 — Gatekeeping Value Protection Documented (Priority: P2)

A developer reading the developer guide or the Divisor team page understands that certain values are protected from modification by AI agents. When an agent cannot meet a quality gate, it stops and reports the conflict rather than weakening the gate. The eight protected categories are listed and explained.

**Why this priority**: Gatekeeping protection is a governance principle that affects how developers interpret agent behavior during reviews. Understanding this principle prevents confusion when an agent stops mid-implementation — the developer needs to know this is by design, not a failure. This is P2 rather than P1 because the blog post contradiction (P1) is more urgent and user-facing.

**Independent Test**: A developer reading the developer guide can find and list the eight categories of protected values and understand what happens when an agent encounters a gate it cannot meet.

**Acceptance Scenarios**:

1. **Given** a developer reading the developer guide, **When** they look for agent behavior constraints, **Then** they find the eight protected value categories and understand that agents stop and report rather than modifying gates.
2. **Given** a developer reading the Divisor team page, **When** they review the Guard persona, **Then** they find the "Gatekeeping Integrity" audit item.
3. **Given** a developer reading the Divisor team page, **When** they review the Adversary persona, **Then** they find the "Gate Tampering" audit item.

---

### User Story 3 — `.gitignore` Management Documented (Priority: P2)

A developer reading about `uf init` understands that it automatically manages `.gitignore` entries. The developer guide explains what entries are added, the append-only behavior, and the idempotency marker. The quick-start page mentions `.gitignore` management as part of `uf setup`.

**Why this priority**: This is a quality-of-life documentation gap. Developers who run `uf init` and notice `.gitignore` changes need to understand that this is expected behavior. Without documentation, they may revert the changes thinking they are unwanted. This is P2 alongside gatekeeping because both are important but neither causes a user-facing contradiction like the persona count issue.

**Independent Test**: A developer reading the developer guide's `uf init` section can find `.gitignore` listed as a managed file with its behavior described (append-only, idempotent, marker comment).

**Acceptance Scenarios**:

1. **Given** a developer reading the developer guide, **When** they review the `uf init` "What Gets Deployed" table, **Then** `.gitignore` is listed as a managed file.
2. **Given** a developer reading the developer guide, **When** they look for `.gitignore` details, **Then** they find the append-only behavior, idempotency marker (`# Unbound Force — managed by uf init`), and the categories of ignored files (`.uf/` runtime data, legacy tool directories).
3. **Given** a developer reading the quick-start page, **When** they review what `uf setup` does, **Then** `.gitignore` management is mentioned alongside project scaffolding.

---

### Edge Cases

- The Divisor team page says "eight" in six different places. All six must be updated. Missing even one creates an internal inconsistency on the same page.
- The blog post (`curator-blog.md`) references "nine personas" — the team page update must make this link-through consistent. The blog post itself is NOT modified.
- Pages that describe the sub-category breakdown ("five review, three content creation") must be updated to "six review, three content creation" or equivalent. The Curator participates in the review phase (it runs during code review alongside the other review personas), so it is counted as a review persona. Three pages have this sub-category breakdown: the-divisor.md, team/\_index.md, and contributing/\_index.md.
- The contributing page's Divisor description must match the team page count.
- The `.gitignore` documentation must explain that `uf init` creates the file if it does not exist — this is different from the append-only behavior for existing files.
- Gatekeeping documentation must not list specific threshold values (those are implementation details that may change). It should describe the categories and the "stop and report" behavior.

## Requirements _(mandatory)_

### Functional Requirements

**Issue #35 — The Curator (4 unique pages, multiple changes to the-divisor.md)**:

- **FR-001**: The Divisor team page (`content/docs/team/the-divisor.md`) MUST update all persona count references from "eight" to "nine" (7 word-level occurrences across 6 lines identified, including the lens count in the "Collective Approval" section).
- **FR-002**: The Divisor team page MUST add a Curator section documenting its role (documentation & content pipeline triage), its five audit checklist items, its restricted bash access (`gh` CLI only), and its triage-not-creation scope.
- **FR-003**: The Divisor team page MUST add The Curator to the Ownership Boundaries table with its exclusive domain.
- **FR-004**: The team index page (`content/docs/team/_index.md`) MUST update the Divisor summary from "eight sub-personas" to "nine sub-personas."
- **FR-005**: The contributing page (`content/docs/contributing/_index.md`) MUST update its Divisor reference from "eight sub-personas" to "nine sub-personas."
- **FR-006**: The common workflows page (`content/docs/getting-started/common-workflows.md`) MUST add The Curator to the code review persona evaluation table.
- **FR-016**: All pages that describe the sub-category breakdown ("five review" / "five for code review" and "three content creation") MUST be updated to "six review" and "three content creation" (or equivalent phrasing reflecting that The Curator is a review-phase persona). Affected pages: the-divisor.md, team/\_index.md, contributing/\_index.md.

**Issue #33 — Gatekeeping Protection (2 affected pages)**:

- **FR-007**: The developer guide (`content/docs/getting-started/developer.md`) MUST add a section documenting the eight categories of gatekeeping-protected values and the "stop and report" agent behavior.
- **FR-008**: The Divisor team page MUST document the Guard's "Gatekeeping Integrity" audit item and the Adversary's "Gate Tampering" audit item.

**Issue #34 — `.gitignore` Management (2 affected pages)**:

- **FR-009**: The developer guide MUST add `.gitignore` to the `uf init` "What Gets Deployed" table or equivalent section.
- **FR-010**: The developer guide MUST document `.gitignore` management behavior: append-only, idempotency via marker comment, file creation if absent, categories of ignored entries (`.uf/` runtime data, legacy tool directories).
- **FR-011**: The quick-start page (`content/docs/getting-started/quick-start.md`) MUST mention `.gitignore` management as part of what `uf setup`/`uf init` handles.

**Cross-cutting**:

- **FR-012**: All content MUST be sourced from upstream PRs (#85 for gatekeeping, #87 for gitignore-init, #89 for Curator) and their associated OpenSpec change artifacts. No capabilities may be fabricated or overstated.
- **FR-013**: The site MUST build successfully (`npm run build`) with no errors after all changes.
- **FR-014**: All updated pages MUST render correctly in both light and dark mode.
- **FR-015**: No page may contain placeholder text, "Coming Soon," or TODO markers after changes.

## Scope & Constraints

### In Scope

- Updating persona count from "eight" to "nine" across all affected pages (4 unique pages identified for Issue #35).
- Adding The Curator persona section to the Divisor team page.
- Adding gatekeeping value protection documentation to the developer guide and Divisor team page.
- Adding `.gitignore` management documentation to the developer guide and quick-start page.
- Updating the code review persona table in common workflows.

### Out of Scope

- Blog posts — the Curator blog post (spec 022) is already published and NOT modified.
- The Curator's agent definition file — that lives in the upstream `unbound-force/unbound-force` repo.
- Changes to the homepage, navigation, or site structure.
- New pages — all changes are updates to existing pages.
- Upstream code changes to `uf init` or the Divisor framework.

## Dependencies & Assumptions

### Dependencies

- **Spec 022 (curator-blog)**: Published a blog post that references "nine personas." This spec resolves the resulting inconsistency on the team page.
- **Spec 020 (dewey-v3-docs-sync)**: Established the pattern of documentation sync specs that update multiple existing pages. This spec follows the same pattern.
- **Upstream PR #85**: Source of truth for gatekeeping value protection rules.
- **Upstream PR #87**: Source of truth for `ensureGitignore` behavior.
- **Upstream PR #89**: Source of truth for The Curator agent and spec 026-documentation-curation.

### Assumptions

- The upstream PRs (#85, #87, #89) are all merged and their changes are final.
- The Divisor team page structure (sections for each persona, ownership boundaries table, lenses list) is stable and does not need restructuring — only content additions and count updates.
- The developer guide's `uf init` section structure is stable and only needs content additions (new table row, new subsection).
- The quick-start page only needs a brief mention of `.gitignore` management, not a detailed section.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: Zero occurrences of "eight" in the context of Divisor/personas/sub-personas remain across the entire `content/` directory after changes.
- **SC-002**: A visitor navigating from the Curator blog post to the Divisor team page finds consistent persona counts (nine) and The Curator documented.
- **SC-003**: A developer can find and list the eight gatekeeping-protected value categories from the developer guide.
- **SC-004**: A developer can find `.gitignore` management documented in the `uf init` section of the developer guide.
- **SC-005**: The site builds successfully (`npm run build`) with zero errors and all updated pages render correctly in both light and dark mode.
