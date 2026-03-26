# Research: Autonomous Define Documentation

**Date**: 2026-03-26
**Feature**: 009-autonomous-define-docs
**Status**: Complete — all decisions resolved

## Research Tasks

### R1: Default Workflow Presentation

**Question**: Should the 1-checkpoint or 2-checkpoint model be presented as the default?

**Decision**: Present the 1-checkpoint model (seed + accept) as the default.

**Rationale**: The design paper (section 8.2) presents the Dewey-enabled workflow as the target state. The 2-checkpoint model is the legacy behavior for projects without Dewey. Presenting autonomous define as default frames Dewey as integral to the swarm's value proposition.

**Alternatives considered**:

- Both models as equal alternatives — creates confusion about which to use
- 2-checkpoint as default with autonomous as "advanced" — undersells Phase 5

### R2: Canonical Terminology for Human Input

**Question**: What should the human's input action be called in autonomous define mode?

**Decision**: Use "seed" as the canonical term.

**Rationale**: The design paper (section 8.2) uses "seed" in the workflow diagram: `HUMAN: seed (select intent) [human]`. Concise, memorable, and clearly distinct from "define."

**Alternatives considered**:

- "Select intent" — clearer but verbose for repeated use
- "Describe" — too generic
- "Request" — implies transactional relationship

### R3: Optional Spec Review Default State

**Question**: Should the optional specification review checkpoint be on or off by default?

**Decision**: Off by default. Documented as a configurable per-workflow option.

**Rationale**: The design paper (section 8.4) describes it as "optional" and "for high-stakes features." Default-on would undermine the 1-checkpoint value proposition.

**Alternatives considered**:

- Default-on with opt-out — reintroduces the checkpoint Phase 5 removes
- Always-off (no option) — design paper explicitly provides it as a safety valve

### R4: Homepage Dewey Card Link Target

**Question**: Should the Dewey homepage card link to GitHub or the docs project page?

**Decision**: Link to the GitHub repository (`https://github.com/unbound-force/dewey`).

**Rationale**: The existing Gaze card links to its GitHub repo. Following the same pattern keeps the homepage consistent.

**Alternatives considered**:

- Link to `/docs/projects/dewey/` — breaks established pattern
- Link to both — adds visual complexity to the card

### R5: Define Stage Execution Mode Label

**Question**: How should the define stage label change in the workflow documentation?

**Decision**: Change from `[human]` to `[swarm]` with inline text explaining the `[human]` fallback.

**Rationale**: The existing workflow uses a clean binary `[human]` / `[swarm]` convention. The design paper (section 8.2) places define in the swarm block. A third label type would break the binary convention.

**Alternatives considered**:

- `[configurable]` — introduces a third mode concept that doesn't exist in code
- `[human/swarm]` — visually cluttered, doesn't convey which is default

### R6: Current Page Content Audit

**Question**: What is the exact current state of each page that needs editing?

**Findings**: Full audit completed. See plan.md "Edit Strategy Per File" section for detailed per-file analysis. Key findings:

- `common-workflows.md` (270 lines): Stage 1 says `[human]`, "Swarm Delegation" says "two human decision points." Both need updating.
- `product-owner.md` (132 lines): "Your Role" describes PO as driving spec creation. Needs reframing around seed interaction.
- `projects/dewey.md` (73 lines): No mention of autonomous define. Needs "Workflow Impact" content.
- `team/dewey.md` (129 lines): Muti-Mind row says "Cross-repo issue discovery, past acceptance criteria, backlog pattern analysis." Needs to mention autonomous specification drafting.
- `getting-started/_index.md` (50 lines): Says "from implementation through review." Should say "from define through review."
- `knowledge.md` (171 lines): Describes Dewey as context enhancement. Should mention it enables autonomous define.
- `projects/_index.md` (15 lines): Lists only Gaze. Dewey missing.
- `layouts/home.html` (342 lines): Only Gaze project card. Dewey missing.
