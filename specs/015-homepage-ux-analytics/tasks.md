# Tasks: Homepage UX Redesign and Analytics

**Input**: Design documents from `/specs/015-homepage-ux-analytics/`

## Format: `[ID] [P?] [Story] Description`

---

## Phase 1: User Story 5 — Google Analytics (Priority: P1)

**Goal**: Add GA4 tracking site-wide before other changes so engagement is measurable.

- [x] T001 [US5] Add `googleAnalytics = "G-969E28NTFN"` to config/\_default/hugo.toml after the `enableRobotsTXT` line. (FR-015, FR-016)

**Checkpoint**: `npm run build` passes. Page source contains gtag script with G-969E28NTFN.

---

## Phase 2: User Story 1 — Hero Section Redesign (Priority: P1)

**Goal**: Swap CTAs, rewrite lead text, update badge, simplify buttons.

- [x] T002 [US1] Replace the hero primary CTA in layouts/home.html: change "Explore on GitHub" (external link) to "Get Started" linking to `/docs/getting-started/quick-start/`. Remove the arrow SVG icon, use plain text button. (FR-001, FR-006)
- [x] T003 [US1] Replace the hero secondary CTA in layouts/home.html: change "View Gaze" (with GitHub SVG) to "Explore on GitHub" linking to `https://github.com/unbound-force`. Remove the GitHub SVG logo, use plain text button with `target="_blank" rel="noopener"`. (FR-002, FR-003, FR-006)
- [x] T004 [US1] Update the hero badge text in layouts/home.html from "Open Source AI Agent Swarm" to "Autonomous Development Pipeline". (FR-005)
- [x] T005 [US1] Update the lead text in content/\_index.md frontmatter to communicate the autonomous pipeline capability. New lead: "AI coding agents are powerful but not self-directing. Unbound Force gives them structure — one command to go from a feature description to tested, reviewed, demo-ready code." (FR-004)

**Checkpoint**: `npm run build` passes. Hero has "Get Started" primary, "Explore on GitHub" secondary, new badge, new lead.

---

## Phase 3: User Story 2 — Feature Card Hierarchy (Priority: P1)

**Goal**: Make /unleash card visually prominent, rewrite headings to be action/benefit-oriented.

- [x] T006 [US2] Restructure the feature cards section in layouts/home.html: extract the /unleash card ("Spec to Demo in One Command") from the 4-column grid and place it as a full-width featured card above the remaining 3 cards. Give the featured card a larger description area and a subtle accent background. The 3 remaining cards sit in a 3-column grid below. (FR-007)
- [x] T007 [US2] Add CSS for the featured card in assets/scss/common/\_custom.scss: a `.feature-card-featured` class with accent border-left or subtle background, larger padding, and wider layout. Ensure dark mode compatibility. (FR-007)
- [x] T008 [US2] Update feature card headings in layouts/home.html: change "Agent Personas" to "A Team That Knows Your Codebase", change "Quality-First" to "Every Change Tested and Reviewed". Keep "Spec to Demo in One Command" and "Open Source" as-is. (FR-008)

**Checkpoint**: `npm run build` passes. /unleash card is visually distinct. Headings are action/benefit-oriented.

---

## Phase 4: User Story 3 — How It Works + Blog Count (Priority: P2)

**Goal**: Add "How It Works" section between hero and features, show 3 blog posts.

- [x] T009 [US3] Add a "How It Works" section to layouts/home.html between the hero section and the features section. Use a 3-column grid with step numbers (1, 2, 3), headings (Specify, Unleash, Finale), and 1-line descriptions. Link each step to the relevant doc page. (FR-009, FR-011)
- [x] T010 [US3] Add CSS for the "How It Works" section in assets/scss/common/\_custom.scss: step number badges (circular, accent-colored), responsive vertical stacking on mobile (`@media max-width: 991.98px`). (FR-011)
- [x] T011 [US3] Change the blog section template in layouts/home.html from `first 1` to `first 3` and update the grid layout from a single-column centered card to a 3-column grid (`col-lg-4` instead of `col-lg-6`). (FR-010)

**Checkpoint**: `npm run build` passes. "How It Works" section visible. 3 blog posts displayed.

---

## Phase 5: User Story 4 — Cleanup (Priority: P3)

**Goal**: Remove sidebar footer duplicate, add benefit framing to project cards, clean up.

- [x] T012 [US4] Disable the sidebar footer section by setting `sectionFooter = false` in config/\_default/params.toml (currently set to `true`). (FR-012)
- [x] T013 [US4] Update the Gaze project card description in layouts/home.html to include benefit framing: explain what it means for the developer's workflow, not just what it does technically. (FR-013)
- [x] T014 [US4] Update the Dewey project card description in layouts/home.html to include benefit framing: explain the benefit of giving agents access to cross-repo context and current API docs. (FR-014)

**Checkpoint**: `npm run build` passes. No duplicate CTA. Project cards have benefit framing.

---

## Phase 6: Verification

- [x] T015 Run `npm run build` and verify zero errors. (FR-017)
- [x] T016 Verify dark mode renders correctly for all new/changed sections. (FR-018)
- [x] T017 Verify Google Analytics script present in rendered page source with G-969E28NTFN.

<!-- spec-review: passed -->
