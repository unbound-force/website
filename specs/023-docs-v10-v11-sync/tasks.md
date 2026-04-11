# Tasks: v0.10.0 / v0.11.0 Documentation Sync

**Input**: Design documents from `/specs/023-docs-v10-v11-sync/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, quickstart.md

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

---

## Phase 1: Setup (Build Verification & Audit)

**Purpose**: Confirm clean baseline before making content changes

- [x] T001 Run `npm run build` and verify zero errors on current branch baseline
- [x] T002 Audit all "eight" occurrences in `content/docs/team/the-divisor.md`, `content/docs/team/_index.md`, and `content/docs/contributing/_index.md` — confirm 6 occurrences in the-divisor.md, 1 in team/\_index.md, 1 in contributing/\_index.md match research.md line/context inventory

**Checkpoint**: Clean build confirmed, stale content locations verified against research.md

---

## Phase 2: User Story 1 — The Curator (Priority: P1)

**Goal**: Update persona count from "eight" to "nine" across 5 pages and add full Curator documentation to the Divisor team page. Resolves blog post ↔ team page contradiction.

**Independent Test**: `grep -rn "eight" content/docs/team/the-divisor.md content/docs/team/_index.md content/docs/contributing/_index.md` returns zero matches in persona-count contexts. The Divisor team page lists nine personas with The Curator documented.

### the-divisor.md (sequential — shared file)

- [x] T003 [US1] Update all 7 persona-count word occurrences of "eight" to "nine" across 6 lines in `content/docs/team/the-divisor.md` (frontmatter description, council structure paragraph ×2 on same line, severity standard, prior learnings, collective approval, how Divisor serves the team) — FR-001
- [x] T004 [US1] Add Curator "Triage Persona" section to `content/docs/team/the-divisor.md` between "Content Creation Personas" and "Exclusive Ownership Boundaries" — include role description (Documentation & Content Pipeline Triage), five audit checklist items, bash access restriction (`gh` CLI only: `gh issue list`, `gh issue create`), scope boundary (triage, not creation), temperature (0.2) — FR-002
- [x] T005 [US1] Add Curator row to the Exclusive Ownership Boundaries table in `content/docs/team/the-divisor.md` with domain "Documentation & content pipeline triage" — FR-003
- [x] T006 [US1] Update the "Collective Approval" section lens count in `content/docs/team/the-divisor.md` from eight lenses to nine and add "documentation triage" to the lens list between "test quality" and "documentation accuracy" (matching Review → Triage → Creation order). Also update any sub-category breakdown text ("five review" → "six review") in the introduction and body text — FR-001, FR-016

### Other pages (parallel — different files)

- [x] T007 [P] [US1] Update Divisor summary in `content/docs/team/_index.md` from "eight sub-personas" to "nine sub-personas", update sub-category breakdown from "five for code review" to "six for code review" (adding The Curator), and add "The Curator" to the parenthetical persona list — FR-004, FR-016
- [x] T008 [P] [US1] Update Divisor description in `content/docs/contributing/_index.md` from "eight sub-personas" to "nine sub-personas", update "Five review personas" to "Six review personas" and add The Curator to the persona enumeration — FR-005, FR-016
- [x] T009 [P] [US1] Add Curator row to the code review persona evaluation table in `content/docs/getting-started/common-workflows.md`: `| **Curator** | Documentation gaps, blog/tutorial opportunities, website issue filing |` — FR-006

**Checkpoint**: Run `npm run build`. Verify zero errors. Grep confirms no stale "eight" in persona-count contexts. Blog post link-through to team page shows consistent "nine personas."

---

## Phase 3: User Story 2 — Gatekeeping Value Protection (Priority: P2)

**Goal**: Document the eight categories of gatekeeping-protected values and the "stop and report" agent behavior. Add Guard and Adversary audit items.

**Independent Test**: A developer reading the developer guide can find and list the eight protected value categories and understand the "stop and report" behavior.

- [x] T010 [P] [US2] Add "Gatekeeping Value Protection" section to `content/docs/getting-started/developer.md` under the Cobalt-Crush persona section after "Feedback Loops" — include brief explanation, eight protected categories (numbered list from research.md), "stop and report" behavior description, note that modifying a gate without human authorization is a CRITICAL-severity constitution violation — FR-007
- [x] T011 [US2] Add "Gatekeeping Integrity" audit item to The Guard section and "Gate Tampering" audit item to The Adversary section in `content/docs/team/the-divisor.md` — FR-008

**Checkpoint**: Run `npm run build`. Verify zero errors. Developer guide gatekeeping section is complete and sourced from upstream PR #85.

---

## Phase 4: User Story 3 — `.gitignore` Management (Priority: P2)

**Goal**: Document `.gitignore` management behavior in `uf init` and mention it in the quick-start.

**Independent Test**: A developer reading the developer guide's `uf init` section finds `.gitignore` listed as a managed file with append-only, idempotent behavior described.

- [x] T012 [P] [US3] Add `.gitignore` row to the "What Gets Deployed" table in `content/docs/getting-started/developer.md` and add a paragraph explaining behavior: append-only (never overwrites existing content), idempotent via marker comment (`# Unbound Force — managed by uf init`), creates `.gitignore` if absent, two categories of ignored entries (`.uf/` runtime data, legacy tool directories) — FR-009, FR-010
- [x] T013 [P] [US3] Add `.gitignore` management mention to the `uf setup`/`uf init` bullet list in `content/docs/getting-started/quick-start.md` — brief mention only, developer guide has details — FR-011

**Checkpoint**: Run `npm run build`. Verify zero errors. Developer guide `uf init` section documents `.gitignore`. Quick-start mentions it.

---

## Phase 5: Polish & Cross-Cutting Concerns

**Purpose**: Final verification across all 6 pages, content accuracy, and build validation

- [x] T014 Run full stale-mention audit: `grep -rn "eight" content/` and verify "eight" only appears in (a) the gatekeeping section (eight protected value categories) and (b) blog post historical references — SC-001, FR-012
- [x] T015 Run `npm run build` — final build verification with all changes applied — FR-013, SC-005
- [x] T016 Cross-reference consistency: verify blog post (`/blog/the-curator-why-documentation-triage-needed-its-own-agent/`) link-through to Divisor team page shows consistent "nine personas" — SC-002
- [x] T017 Verify no placeholder text, "Coming Soon," or TODO markers exist in any of the 6 modified pages — FR-015
- [x] T018 Run quickstart.md verification steps (Sections 2-5): persona count consistency check, visual verification of all 6 pages, dark mode verification, cross-reference consistency — FR-014, SC-005

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (Setup)**: No dependencies — start immediately
- **Phase 2 (US1 — Curator)**: Depends on Phase 1 completion
- **Phase 3 (US2 — Gatekeeping)**: Depends on Phase 2 completion (shares `the-divisor.md`)
- **Phase 4 (US3 — Gitignore)**: Depends on Phase 3 completion (shares `developer.md`)
- **Phase 5 (Polish)**: Depends on all previous phases

### Parallel Opportunities

- **Phase 2**: T007, T008, T009 are [P] — they touch different files (`_index.md`, `contributing/_index.md`, `common-workflows.md`) and can run in parallel. T003→T004→T005→T006 are sequential (all modify `the-divisor.md`).
- **Phase 3**: T010 and T011 are on different files (`developer.md` vs `the-divisor.md`) — T010 is [P].
- **Phase 4**: T012 and T013 are on different files (`developer.md` vs `quick-start.md`) — both are [P].

### File Contention Map

| File                     | Tasks (in order)             |
| ------------------------ | ---------------------------- |
| `the-divisor.md`         | T003, T004, T005, T006, T011 |
| `team/_index.md`         | T007                         |
| `contributing/_index.md` | T008                         |
| `common-workflows.md`    | T009                         |
| `developer.md`           | T010, T012                   |
| `quick-start.md`         | T013                         |

---

## Notes

- Total: 18 tasks across 5 phases, 6 files, ~85 lines of Markdown changes
- No new files created — all edits to existing pages
- No tests to write — validation is `npm run build` + visual verification
- Content must be sourced from upstream PRs #85, #87, #89 (FR-012) — no fabrication
- The word "eight" will correctly remain in the gatekeeping section (eight protected value categories) — only persona-count "eight" references are updated

<!-- spec-review: passed -->
