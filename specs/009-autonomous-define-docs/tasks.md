# Tasks: Autonomous Define Documentation

**Input**: Design documents from `/specs/009-autonomous-define-docs/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Phase 1: Setup

**Purpose**: No project initialization needed — all target files already exist. This phase verifies the environment is ready for content editing.

- [x] T001 Run `npm run build` to verify the site builds cleanly before any changes

---

## Phase 2: Foundational

**Purpose**: No foundational/blocking prerequisites for this feature. All tasks edit independent files and can proceed directly to user story phases after the setup build check passes.

**Checkpoint**: Build passes — content editing can begin.

---

## Phase 3: User Story 1 - Updated Hero Lifecycle Workflow (Priority: P1)

**Goal**: Update the Common Workflows page so the hero lifecycle reflects both the 1-checkpoint model (seed + accept) and the 2-checkpoint model (define + accept), with autonomous define as the default for Dewey-enabled projects.

**Independent Test**: Navigate to the Common Workflows page via `npm run dev`, verify Stage 1 describes both modes, the seed interaction is explained, the optional spec review checkpoint is documented, and the Swarm Delegation section describes both models.

### Implementation for User Story 1

- [x] T002 [US1] Update Stage 1 heading from `[human]` to `[swarm]` and rewrite the stage body to describe the seed interaction (human provides 1-2 sentences of intent, Muti-Mind creates spec using Dewey context) as the default mode, with a "Manual Define Mode" subsection documenting the `[human]` fallback for projects without Dewey in `content/docs/getting-started/common-workflows.md`
- [x] T003 [US1] Add an "Optional: Specification Review" subsection between Stage 1 (Define) and Stage 2 (Implement) documenting the configurable spec review checkpoint — when to enable it (high-stakes features, unfamiliar domains), what to evaluate (intent alignment, scope accuracy), and that it is off by default in `content/docs/getting-started/common-workflows.md`
- [x] T004 [US1] Rewrite the "Swarm Delegation" section to describe both models: the 1-checkpoint model (seed + accept) as the default for Dewey-enabled projects and the 2-checkpoint model (define + accept) as the fallback, replacing the current text that says "two human decision points" in `content/docs/getting-started/common-workflows.md`
- [x] T005 [US1] Update the "Knowledge Context" section to emphasize Dewey's role in enabling autonomous define — not just providing context during human-driven define, but enabling the swarm to draft specifications autonomously in `content/docs/getting-started/common-workflows.md`

**Checkpoint**: Common Workflows page accurately describes both workflow models. `npm run build` passes. This is the canonical reference that all other pages must be consistent with.

---

## Phase 4: User Story 2 - Updated Product Owner Guide (Priority: P2)

**Goal**: Update the Product Owner guide so the PO understands their new interaction model: seed (express intent in 1-2 sentences) replaces the human-driven define stage as the default.

**Independent Test**: Navigate to the Product Owner guide via `npm run dev`, verify Stage 1 describes the seed interaction with examples, the optional spec review checkpoint is documented, and Stage 5 (Accept) is unchanged.

### Implementation for User Story 2

- [x] T006 [US2] Rewrite "Your Role in the Hero Lifecycle" > "Stage 1: Define" around the seed interaction — the PO provides 1-2 sentences of intent, Muti-Mind uses Dewey to expand into a full specification. Include a "Manual Define Mode" note for projects without Dewey in `content/docs/getting-started/product-owner.md`
- [x] T007 [US2] Add 3-4 seed examples showing what effective seeds look like (e.g., "Fix the authentication timeout bug", "Add CSV export for the dashboard", "Refactor the webhook handler to support retry logic") in `content/docs/getting-started/product-owner.md`
- [x] T008 [US2] Add an "Optional: Specification Review" subsection under Stage 1 explaining when to enable the spec review checkpoint (high-stakes features, unfamiliar domains) and what to look for (intent alignment, scope accuracy — not implementation details) in `content/docs/getting-started/product-owner.md`

**Checkpoint**: Product Owner guide accurately describes the seed interaction and spec review checkpoint. PO can write an effective seed after reading the examples.

---

## Phase 5: User Story 3 - Updated Dewey Impact Documentation (Priority: P3)

**Goal**: Update the Dewey project page and team page to connect Dewey's capabilities to the autonomous define workflow outcome — Dewey doesn't just provide better search, it enables a workflow shift from 2 checkpoints to 1.

**Independent Test**: Navigate to both Dewey pages via `npm run dev`, verify autonomous define is mentioned as a key capability and Muti-Mind's Dewey usage entry reflects autonomous specification drafting.

### Implementation for User Story 3

- [x] T009 [P] [US3] Add a "Workflow Impact" section after "Key Features" on the Dewey project page explaining that Dewey enables autonomous specification creation — reducing human checkpoints from 2 to 1 by providing Muti-Mind with sufficient cross-repo context to draft specs without human intervention in `content/docs/projects/dewey.md`
- [x] T010 [P] [US3] Update Muti-Mind's row in the "How Heroes Use Dewey" table to replace "Cross-repo issue discovery, past acceptance criteria, backlog pattern analysis" with content that includes autonomous specification drafting (retrieve context, draft spec, self-clarify, validate against history) in `content/docs/team/dewey.md`

**Checkpoint**: Both Dewey pages connect features to workflow impact. Visitors understand that Dewey enables a fundamental workflow shift, not just better search.

---

## Phase 6: User Story 4 - Corrected Cross-Site References (Priority: P4)

**Goal**: Update secondary reference pages so they are consistent with the canonical workflow documentation on the Common Workflows page and do not contradict the 1-checkpoint model.

**Independent Test**: Read each updated page and verify any mention of the hero lifecycle or Dewey's role is consistent with the Common Workflows page (US1).

### Implementation for User Story 4

- [x] T011 [P] [US4] Update the swarm autonomy description on the "What is Unbound Force?" page — change "from implementation through review" to reflect that the swarm runs autonomously from define through review (when Dewey is configured) in `content/docs/getting-started/_index.md`
- [x] T012 [P] [US4] Add a sentence in the "What Dewey Does" section of the knowledge retrieval guide connecting Dewey to autonomous define — Dewey provides sufficient context for Muti-Mind to draft specifications without human intervention, reducing human checkpoints from 2 to 1 in `content/docs/getting-started/knowledge.md`
- [x] T013 [P] [US4] Add a Dewey entry under "Current Projects" in the projects section index, following the Gaze entry pattern (H3 heading with link, 1-2 sentence description of what Dewey does) in `content/docs/projects/_index.md`

**Checkpoint**: All secondary reference pages are consistent with the canonical workflow docs. No page contradicts another.

---

## Phase 7: User Story 5 - Homepage Dewey Visibility (Priority: P5)

**Goal**: Add Dewey to the homepage so first-time visitors discover both major projects (Gaze and Dewey), not just one.

**Independent Test**: Load the homepage via `npm run dev`, verify the Projects section contains both a Gaze card and a Dewey card, and the Dewey card links to `https://github.com/unbound-force/dewey`.

### Implementation for User Story 5

- [x] T014 [US5] Add a Dewey project card in the Projects section of the homepage, following the existing Gaze card pattern (same HTML structure: badge, label, title, description), linking to `https://github.com/unbound-force/dewey` in `layouts/home.html`

**Checkpoint**: Homepage displays both project cards side by side. Dewey card links to the GitHub repo.

---

## Phase 8: Polish & Cross-Cutting Concerns

**Purpose**: Verification across all updated pages to ensure consistency and build integrity.

- [ ] T015 Run `npm run build` to verify all changes compile without errors
- [ ] T016 Run `npm run dev` and visually verify all 8 updated pages render correctly in both light and dark mode
- [ ] T017 Verify all cross-page links in updated content resolve to valid pages (click-through check on Common Workflows, Product Owner, Dewey project, Dewey team, knowledge guide, projects index, homepage)

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — verifies build baseline
- **User Story 1 (Phase 3)**: Depends on Setup — SHOULD complete first as the canonical reference
- **User Story 2 (Phase 4)**: Can start after Setup; SHOULD reference US1's content for consistency
- **User Story 3 (Phase 5)**: Can start after Setup; independent of US1 and US2 (different files)
- **User Story 4 (Phase 6)**: Can start after Setup; SHOULD reference US1's content for consistency
- **User Story 5 (Phase 7)**: Can start after Setup; independent of all other stories (different file)
- **Polish (Phase 8)**: Depends on all user stories being complete

### User Story Dependencies

- **US1 (P1)**: No dependencies on other stories. SHOULD complete first — it establishes the canonical workflow description that US2 and US4 must be consistent with.
- **US2 (P2)**: Logically depends on US1 (PO guide references the workflow model defined in US1). Can technically start in parallel since it edits a different file, but content should align with US1.
- **US3 (P3)**: Fully independent — edits `projects/dewey.md` and `team/dewey.md`, which are not touched by other stories. Can run in parallel with US1.
- **US4 (P4)**: Logically depends on US1 (cross-references must match the canonical workflow page). Edits 3 different files, all independent from other stories.
- **US5 (P5)**: Fully independent — edits `layouts/home.html`, which is not touched by other stories. Can run in parallel with any story.

### Parallel Opportunities

- **T009 and T010** (US3): Edit different files, can run in parallel
- **T011, T012, and T013** (US4): Edit different files, can run in parallel
- **US3 and US5**: Edit completely different files from US1/US2/US4, can run in parallel with any other story
- **All user stories** can technically run in parallel (different files), but US1 should complete first so US2 and US4 can reference its content for consistency

---

## Parallel Example: User Story 3 + User Story 5

```
# These stories edit completely different files and can run simultaneously:
Task: "Add Workflow Impact section to content/docs/projects/dewey.md"
Task: "Update Muti-Mind row in content/docs/team/dewey.md"
Task: "Add Dewey project card to layouts/home.html"
```

---

## Parallel Example: User Story 4

```
# All three cross-reference updates edit different files:
Task: "Update getting-started/_index.md"
Task: "Update knowledge.md"
Task: "Update projects/_index.md"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup (build check)
2. Complete Phase 3: User Story 1 (Common Workflows page)
3. **STOP and VALIDATE**: The canonical workflow reference is correct
4. This single page update delivers the core value — all other pages reference it

### Incremental Delivery

1. US1 (Common Workflows) → canonical reference established
2. US2 (Product Owner guide) → PO interaction model updated
3. US3 (Dewey pages) → impact documentation complete
4. US4 (Cross-references) → full site consistency
5. US5 (Homepage) → Dewey discoverable from homepage
6. Polish → build check, visual verification, link audit

### Recommended Execution Order

Sequential: US1 → US2 → US3 → US4 → US5 → Polish

This order ensures each subsequent story can reference the canonical content established by US1 for consistency.

---

## Notes

- All tasks edit existing files — no new files are created
- No test tasks included (spec does not request tests; validation is manual via `npm run build` and `npm run dev`)
- Content for all changes is derived from the Dewey Design Paper (sections 8.1-8.4) and Orchestration Plan (Phase 5, section 5.1) — do not fabricate capabilities
- The term "seed" is the canonical term for the human's input in autonomous define mode (research decision R2)
- The 1-checkpoint model (seed + accept) is the default; 2-checkpoint (define + accept) is the fallback (research decision R1)
- The optional spec review is off by default (research decision R3)
