# Implementation Plan: Autonomous Define Documentation

**Branch**: `009-autonomous-define-docs` | **Date**: 2026-03-26 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/009-autonomous-define-docs/spec.md`

## Summary

Document the Phase 5 Autonomous Define workflow on the Unbound Force website. The hero lifecycle shifts from 2 human checkpoints (define + accept) to 1 (seed + accept) when Dewey is configured. This requires updating the Common Workflows page (canonical workflow reference), the Product Owner guide (PO's interaction model), Dewey project and team pages (impact documentation), cross-site references (consistency), the projects section index (missing Dewey listing), and the homepage (missing Dewey project card).

All changes are Markdown content edits or Hugo template edits. No new dependencies, no new pages, no custom CSS or JavaScript.

## Technical Context

**Language/Version**: Hugo (Go-based SSG) via `@thulite` npm packages; Node.js >= 20.11.0; Go >= 1.23
**Primary Dependencies**: `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`
**Storage**: N/A (static Markdown files)
**Testing**: Manual — `npm run build` (zero errors), `npm run dev` (visual verification in light/dark mode)
**Target Platform**: GitHub Pages (static HTML)
**Project Type**: Static documentation website (Hugo + Doks theme)
**Performance Goals**: N/A (static site, no runtime performance concerns)
**Constraints**: Zero-waste mandate (no placeholder content), content accuracy (derived from design paper + orchestration plan), minimal footprint (Markdown over custom HTML)
**Scale/Scope**: 8 files modified (6 Markdown content pages + 1 Hugo template + 1 section index)

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### I. Content Accuracy — PASS

All content changes are derived from two authoritative source documents:

- **Dewey Design Paper** (sections 8.1–8.4): defines the seed → accept workflow, the optional spec review checkpoint, and Muti-Mind's 5-step autonomous define process
- **Dewey Orchestration Plan** (Phase 5, section 5.1): explicitly lists the website tasks

No capabilities are fabricated. The design paper describes Phase 5 as "NOT STARTED" in the meta repo, but the website documents the target state (to be published alongside or after implementation). The spec's edge case section addresses this timing explicitly.

### II. Minimal Footprint — PASS

All changes are Markdown content edits to existing pages. The only non-Markdown change is adding a Dewey project card to `layouts/home.html`, which follows the existing Gaze card pattern exactly (same HTML structure, no new CSS or JavaScript). No new pages are created. No new dependencies. No custom layouts or shortcodes.

### III. Visitor Clarity — PASS

The workflow documentation becomes clearer by:

- Explicitly labeling execution modes (`[human]` vs `[swarm]`) on the define stage
- Describing both workflow models so visitors understand which applies to their project
- Adding seed examples to the PO guide so the interaction model is immediately understandable
- Adding Dewey to the homepage and projects index so visitors discover it through normal navigation (currently hidden behind direct links only)

The information hierarchy remains intact: homepage → projects → detailed docs. The Dewey project card makes Dewey discoverable from the homepage (currently requires 2+ clicks from homepage to find Dewey).

**Gate result: ALL PASS. Proceeding to Phase 0.**

## Project Structure

### Documentation (this feature)

```text
specs/009-autonomous-define-docs/
├── spec.md              # Feature specification
├── plan.md              # This file
├── research.md          # Phase 0: content strategy research
├── checklists/
│   └── requirements.md  # Spec quality checklist (already created)
└── tasks.md             # Phase 2 output (/speckit.tasks command)
```

### Source Code (repository root)

```text
content/docs/getting-started/
├── _index.md              # US4: update swarm autonomy description
├── common-workflows.md    # US1: primary workflow reference update
├── product-owner.md       # US2: PO interaction model update
└── knowledge.md           # US4: add autonomous define mention

content/docs/projects/
├── _index.md              # US4: add Dewey to current projects listing
└── dewey.md               # US3: add autonomous define as key capability

content/docs/team/
└── dewey.md               # US3: update Muti-Mind's Dewey usage entry

layouts/
└── home.html              # US5: add Dewey project card
```

**Structure Decision**: No new files or directories. All changes are edits to existing files. The project structure is unchanged.

## Research

### Content Strategy Decisions

**Decision**: Present the 1-checkpoint model (seed + accept) as the default, with the 2-checkpoint model (define + accept) as a fallback for projects without Dewey.

**Rationale**: The design paper (section 8.2) presents the Dewey-enabled workflow as the target state. The 2-checkpoint model is the legacy behavior that applies only when Dewey is not configured. Presenting autonomous define as the default frames Dewey as integral to the swarm's value proposition rather than an optional add-on.

**Alternatives considered**: (1) Present both models as equal alternatives — rejected because it creates confusion about which to use and doesn't reflect the design intent. (2) Present 2-checkpoint as default with autonomous as "advanced" — rejected because it undersells the Phase 5 value and makes the simpler workflow seem harder to achieve.

---

**Decision**: Use "seed" as the canonical term for the human's input in autonomous define mode.

**Rationale**: The design paper (section 8.2) uses "seed" in the workflow diagram: `HUMAN: seed (select intent) [human]`. The parenthetical "select intent" clarifies what "seed" means but "seed" is the label. It's concise, memorable, and clearly distinct from "define" (the stage name).

**Alternatives considered**: "Select intent" — clearer but verbose for repeated use. "Describe" — too generic. "Request" — implies a transactional relationship rather than a creative one.

---

**Decision**: The optional spec review checkpoint is documented as a configurable per-workflow option, not a default.

**Rationale**: The design paper (section 8.4) describes it as "optional" and "for high-stakes features." Making it default would undermine the 1-checkpoint value proposition. The documentation should explain when to enable it (unfamiliar domains, high business risk) and what to evaluate (intent alignment, scope accuracy).

**Alternatives considered**: Default-on with opt-out — rejected because it reintroduces a human checkpoint that Phase 5 is designed to remove. Always-off — rejected because the design paper explicitly provides it as a safety valve.

---

**Decision**: The Dewey homepage card links to the GitHub repository (not the project docs page).

**Rationale**: The existing Gaze card links to `https://github.com/unbound-force/gaze`. Following the same pattern keeps the homepage consistent. Visitors who want details can navigate to the docs from there. The project page is already linked from the docs sidebar.

**Alternatives considered**: Link to `/docs/projects/dewey/` — would break the established pattern where homepage project cards link to GitHub repos. Link to both — would add visual complexity to the card.

---

**Decision**: The define stage label changes from `[human]` to `[swarm]` with a note about the `[human]` fallback, rather than showing a new label like `[configurable]`.

**Rationale**: The existing workflow uses a clean `[human]` / `[swarm]` labeling convention. The design paper (section 8.2) shows the define stage as part of the swarm block. Adding a third label type would break the binary convention. Instead, the stage shows `[swarm]` with inline text explaining the `[human]` fallback.

**Alternatives considered**: `[configurable]` label — rejected because it introduces a third execution mode concept that doesn't exist in the code. Dual labels `[human/swarm]` — rejected because it's visually cluttered and doesn't convey which is default.

---

### Edit Strategy Per File

**`common-workflows.md`** (US1 — heaviest edit):

- Stage 1 heading changes from `[human]` to `[swarm]`
- Stage 1 body rewritten to describe seed interaction first, with a subsection for the human-driven fallback
- New subsection for the optional spec review checkpoint (between stages 1 and 2)
- "Swarm Delegation" section rewritten to describe both models with the 1-checkpoint model as default
- "Knowledge Context" section updated to emphasize Dewey's role in enabling autonomous define

**`product-owner.md`** (US2 — medium edit):

- "Your Role in the Hero Lifecycle" > Stage 1 rewritten around the seed interaction
- Add 3-4 seed examples showing what good seeds look like
- Add guidance on the optional spec review checkpoint
- Stage 5 (Accept) unchanged — acceptance decisions are the same regardless of define mode

**`projects/dewey.md`** (US3 — light edit):

- Add a brief "Workflow Impact" or similar section after "Key Features" connecting Dewey's capabilities to the autonomous define outcome

**`team/dewey.md`** (US3 — light edit):

- Update Muti-Mind's row in the "How Heroes Use Dewey" table to mention autonomous specification drafting

**`getting-started/_index.md`** (US4 — minimal edit):

- Update the sentence about swarm autonomy to include the define stage

**`knowledge.md`** (US4 — light edit):

- Add a sentence in the "What Dewey Does" section connecting Dewey to autonomous define

**`projects/_index.md`** (US4 — light edit):

- Add a Dewey entry under "Current Projects" following the Gaze pattern

**`layouts/home.html`** (US5 — medium edit):

- Add a Dewey project card in the projects section following the Gaze card pattern
- Update grid layout from `col-lg-6` (single card) to `col-lg-6` (two cards side by side)

## Complexity Tracking

No constitution violations. No complexity justifications needed.
