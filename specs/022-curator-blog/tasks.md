# Tasks: Introducing The Curator Blog Post

**Input**: Design documents from `/specs/022-curator-blog/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, quickstart.md

**Tests**: No automated tests. Validation is manual: `npm run build` (zero errors) + visual verification in `npm run dev`.

**Organization**: Tasks are grouped by user story. All tasks modify a single file (`content/blog/curator-blog.md`), so there are no parallel opportunities — tasks are strictly sequential.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

---

## Phase 1: Setup

**Purpose**: Verify the build environment is healthy before creating new content.

- [x] T001 Run `npm run build` and confirm zero errors — baseline build health before changes

**Checkpoint**: Build passes. No pre-existing issues that could be confused with regressions from the new post.

---

## Phase 2: User Story 1 — The Curator's Role and Why It Exists (Priority: P1) 🎯 MVP

**Goal**: Write the blog post with the three-section narrative structure from issue #36. A reader with no prior knowledge of Unbound Force understands the documentation drift problem and how The Curator addresses it. The post is self-contained.

**Independent Test**: The post reads as a coherent narrative from problem → solution → interesting design insights. No CLI syntax or agent configuration is duplicated from docs.

### Implementation for User Story 1

- [x] T002 [US1] Create `content/blog/curator-blog.md` with frontmatter: title (`"Introducing The Curator — Automated Documentation Governance for AI Agent Teams"`), description, lead, slug (`introducing-the-curator`), date (`2026-04-11T00:00:00+00:00`), draft (`false`), weight (`56`), toc (`true`), categories (`["Engineering"]`), tags (`["divisor", "documentation", "code-review", "agents"]`), contributors (`["Unbound Force"]`). Match format from existing blog posts per research item R6.
- [x] T003 [US1] Write Section 1 — "The Problem: Documentation Drift" in `content/blog/curator-blog.md`. Describe the documentation drift problem: AI agents ship features without documentation updates, and without a dedicated checker these gaps accumulate silently. Make the problem relatable to anyone using AI coding agents — the gap between code velocity and documentation velocity. Per FR-003, FR-012, spec US1 acceptance scenario 2.
- [x] T004 [US1] Write Section 2 — "The Solution: A Dedicated Triage Agent" in `content/blog/curator-blog.md`. Explain The Curator's role: the 9th Divisor persona, runs during every code review, detects documentation gaps using file-path heuristics, and auto-files GitHub issues to track them. Explain the triage model — The Curator identifies gaps and files issues, while The Scribe (tech docs), The Herald (blog), and The Envoy (communications) handle content creation. State the persona count clearly: 9th persona joining 5 review + 3 content creation. Per FR-004, FR-005, FR-011, FR-013, research items R1, R2, R4, R5.
- [x] T005 [US1] Write Section 3 — "The Interesting Bits" in `content/blog/curator-blog.md`. Cover three design insights: (1) **Bash access restriction** — first Divisor agent with shell access, restricted to `gh` CLI only, with The Adversary's "Gate Tampering" check enforcing compliance. Describe this as a trust architecture pattern: minimum capability + peer verification. (2) **The bootstrap paradox** — The Curator cannot review the PR that creates it because it does not exist until that PR merges. Describe what actually happened during PR #89: reviewed by existing 8 personas, The Curator's first review was the PR after its own creation. Frame as a real engineering story, not a hypothetical. (3) **Triage vs creation separation** — The Curator files the ticket, The Scribe writes the docs, The Herald writes the blog post. Detection needs `read` + `bash`; creation needs `write` + `edit`. Combining both would violate least privilege. Per FR-006, FR-007, FR-013, research items R2, R3, R5.
- [x] T006 [US1] Write a brief closing paragraph in `content/blog/curator-blog.md`. Acknowledge The Curator's heuristic limitations honestly — file-path matching, not semantic understanding; a first-pass filter, not an oracle. Direct readers to the [Divisor team page](/docs/team/the-divisor/) for the full persona roster and the [getting-started guide](/docs/getting-started/developer/) for setup. No time-sensitive language. Per FR-008, FR-009, FR-010, FR-013, research item R8, quickstart.md D5.

**Checkpoint**: The three-section blog post is complete. The narrative flows from problem → solution → design insights. All content is sourced from research.md (PR #89, spec 026, agent definition). No CLI syntax is inlined. Heuristic limitations are acknowledged.

---

## Phase 3: User Story 2 — Technical Design Insights (Priority: P1)

**Goal**: Review and polish the post to ensure the three design insights (bash restriction, bootstrap paradox, triage/creation separation) are clearly communicated and independently valuable. A technical reader can identify and articulate at least two patterns they could apply to their own agent architectures.

**Independent Test**: A technical reader can identify the trust architecture pattern (bash restriction + peer enforcement) and the triage/creation separation from the post without needing external context.

### Implementation for User Story 2

- [x] T007 [US2] Review Section 3 (The Interesting Bits) in `content/blog/curator-blog.md` for technical clarity. Verify: (1) the bash restriction is framed as a trust architecture pattern — minimum capability + peer verification, not just a configuration detail, (2) the bootstrap paradox is told as a concrete story from PR #89, not a hypothetical, (3) the triage/creation separation is explained with the capability rationale (read+bash vs write+edit). Refine prose as needed. Per US2 acceptance scenarios.
- [x] T008 [US2] Verify The Curator's capabilities are not overclaimed in `content/blog/curator-blog.md`. Check: no claims of "semantic understanding," "intelligent detection," or similar. The Curator is described as using heuristic file-path matching. The limitations acknowledgment in the closing paragraph is clear and honest. Per FR-013, quickstart.md V9, research item R8.

**Checkpoint**: Design insights are clearly communicated. A technical reader can identify at least two reusable patterns. No overclaiming.

---

## Phase 4: User Story 3 — Links to Documentation and Context (Priority: P2)

**Goal**: Verify all internal links resolve to valid pages. CLI syntax and agent configuration are linked, not duplicated.

**Independent Test**: Every internal link in the post resolves. No fenced code blocks contain agent configuration YAML, `gh` CLI commands, or heuristic path patterns.

### Implementation for User Story 3

- [x] T009 [US3] Verify all internal links in `content/blog/curator-blog.md` resolve to valid pages. Required links per quickstart.md V4: `/docs/team/the-divisor/` (Divisor team page), `/docs/getting-started/common-workflows/` (common workflows / code review), `/docs/getting-started/developer/` (developer getting-started guide). Fix any broken or missing links. Per FR-008, US3 acceptance scenarios.
- [x] T010 [US3] Verify no fenced code blocks in `content/blog/curator-blog.md` contain agent frontmatter YAML, `gh issue create` commands, or heuristic path patterns. Command names and concepts may appear in prose but implementation syntax must link to docs. Per FR-009, quickstart.md V8.

**Checkpoint**: All internal links resolve. No duplicated configuration or CLI syntax. Readers can follow links to authoritative reference docs.

---

## Phase 5: Polish & Verification

**Purpose**: Final quality checks across all user stories before merge.

- [x] T011 Verify word count of `content/blog/curator-blog.md` body text (excluding frontmatter) is 1200–1800 words. Adjust if outside range. Per spec constraint, quickstart.md V6.
- [x] T012 Verify no time-sensitive language in `content/blog/curator-blog.md`: no "just shipped," "this week," "recently," "now available," or similar phrases. The frontmatter `date` field provides temporal context. Per FR-010, quickstart.md V7.
- [x] T013 Run `npm run build` and confirm zero errors with the new blog post included. Per FR-014, quickstart.md V1.
- [x] T014 Run `npm run dev` and visually verify `content/blog/curator-blog.md` renders correctly: title, lead text, table of contents, three sections with H2 headings, internal links clickable. Verify both light and dark mode. Per FR-015, quickstart.md V2, V3.

**Checkpoint**: Post is publication-ready. Build passes, visual rendering is correct, all quality checks pass.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — confirms build health
- **US1 (Phase 2)**: Depends on Phase 1 — creates the blog post file and writes all content
- **US2 (Phase 3)**: Depends on Phase 2 — reviews and polishes design insight sections
- **US3 (Phase 4)**: Depends on Phase 2 — verifies links and no duplicated syntax
- **Polish (Phase 5)**: Depends on Phases 2, 3, 4 — final quality gate

### Parallel Opportunities

**None.** All tasks modify or review a single file (`content/blog/curator-blog.md`). Tasks must be executed sequentially.

### Execution Strategy

Sequential, single-file workflow:

1. Phase 1: Verify build health (T001)
2. Phase 2: Write the complete blog post (T002–T006)
3. Phase 3: Polish design insight clarity (T007–T008)
4. Phase 4: Verify links and no syntax duplication (T009–T010)
5. Phase 5: Final quality checks (T011–T014)

---

## Notes

- All content is sourced from research.md (PR #89, spec 026, agent definition file). No fabrication.
- The post follows the same pattern established by spec 021 (dewey-curator-blog): narrative blog post that links to docs instead of duplicating reference material.
- Agent configuration syntax, `gh` CLI commands, and heuristic rules live in the agent definition file and docs. The blog post describes concepts and design decisions only.
- Commit after each phase checkpoint, not after individual tasks.

<!-- spec-review: passed -->
