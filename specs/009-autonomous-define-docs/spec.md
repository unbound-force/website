# Feature Specification: Autonomous Define Documentation

**Feature Branch**: `009-autonomous-define-docs`  
**Created**: 2026-03-26  
**Status**: Draft  
**Input**: User description: "Document the Phase 5 Autonomous Define workflow changes for the website — update common workflows, product owner guide, and related pages to reflect the new seed-to-accept single-checkpoint model enabled by Dewey"

## User Scenarios & Testing _(mandatory)_

### User Story 1 - Updated Hero Lifecycle Workflow (Priority: P1)

A visitor reads the Common Workflows page to understand how the Unbound Force hero lifecycle works. They need to see both the current 2-checkpoint model (define + accept) and the new 1-checkpoint model (seed + accept) where the define stage can run autonomously as part of the swarm, powered by Dewey's semantic context. The documentation clearly explains when each model applies, what the human's role is in each, and how to configure the workflow mode.

**Why this priority**: The Common Workflows page is the canonical reference for the hero lifecycle. If this page is wrong or incomplete, every other page that references the workflow is misleading. This is the foundation all other changes build on.

**Independent Test**: Can be fully tested by navigating to the Common Workflows page and verifying that both workflow models are described, the define stage shows its configurable execution mode, the optional spec review checkpoint is documented, and the "Swarm Delegation" section accurately reflects both the 1-checkpoint and 2-checkpoint models.

**Acceptance Scenarios**:

1. **Given** a visitor on the Common Workflows page, **When** they read the "New Feature (End-to-End)" section, **Then** Stage 1 (Define) describes both the human-driven mode and the swarm-driven mode with clear labels for each.
2. **Given** a visitor reading Stage 1, **When** the define stage is in swarm mode, **Then** the documentation explains the "seed" interaction — the human provides 1-2 sentences of intent and the swarm handles specification creation.
3. **Given** a visitor reading the workflow, **When** autonomous define is described, **Then** an optional specification review checkpoint between define and implement is documented as a configurable safeguard.
4. **Given** a visitor reading the "Swarm Delegation" section, **When** they look for the summary, **Then** they see both models: the 2-checkpoint model (define + accept) and the 1-checkpoint model (seed + accept), with the 1-checkpoint model presented as the default for Dewey-enabled projects.

---

### User Story 2 - Updated Product Owner Guide (Priority: P2)

A Product Owner reads their role-specific guide to understand their responsibilities in the hero lifecycle. They need to see that their role has evolved: instead of driving specification creation (writing specs, answering clarification questions), their primary interaction is now a lightweight "seed" — expressing intent in 1-2 sentences. The guide explains what a good seed looks like, when to use the optional spec review checkpoint, and how acceptance decisions work the same as before.

**Why this priority**: The Product Owner guide directly describes the human's interaction model, which is the part that changes most in Phase 5. If POs don't understand their new (simpler) role, the workflow improvement is invisible.

**Independent Test**: Can be fully tested by navigating to the Product Owner guide and verifying that the "Your Role in the Hero Lifecycle" section describes the seed interaction, provides examples of good seeds, documents the optional spec review checkpoint, and retains the existing accept stage content unchanged.

**Acceptance Scenarios**:

1. **Given** a Product Owner reading their guide, **When** they reach "Your Role in the Hero Lifecycle," **Then** Stage 1 describes the seed interaction as the default, with the human-driven define mode available as a configuration option.
2. **Given** a Product Owner reading the seed interaction description, **When** they look for guidance, **Then** the guide includes examples of what a good seed looks like (1-2 sentence intent descriptions) and explains how Muti-Mind uses Dewey to expand the seed into a full specification.
3. **Given** a Product Owner considering the optional spec review, **When** they read about it, **Then** the guide explains when to enable it (high-stakes features, unfamiliar domains) and what to look for (intent alignment, scope accuracy — not implementation details).

---

### User Story 3 - Updated Dewey Impact Documentation (Priority: P3)

A visitor exploring the Dewey project page or team page wants to understand Dewey's impact on the overall workflow. They need to see that Dewey doesn't just provide better search results — it enables a fundamental workflow shift from 2 human checkpoints to 1. The Dewey pages explain this impact clearly, connecting Dewey's semantic context capabilities to the concrete outcome of autonomous specification.

**Why this priority**: Dewey's value proposition is significantly strengthened by Phase 5. Without this update, the Dewey pages undersell the tool's impact — they describe features (semantic search, content sources) but not the workflow transformation those features enable.

**Independent Test**: Can be fully tested by navigating to the Dewey project page and team page and verifying that autonomous define is mentioned as a key capability, with a clear explanation of how Dewey's semantic context enables Muti-Mind to draft specifications without human intervention.

**Acceptance Scenarios**:

1. **Given** a visitor on the Dewey project page, **When** they read about what Dewey does, **Then** they see that Dewey enables autonomous specification creation as a key impact, reducing human checkpoints from 2 to 1.
2. **Given** a visitor on the Dewey team page, **When** they read the "How Heroes Use Dewey" section, **Then** Muti-Mind's entry reflects autonomous specification drafting (retrieve context, draft spec, self-clarify, validate against history) — not just "better-informed decisions."

---

### User Story 4 - Corrected Cross-Site References (Priority: P4)

A visitor browsing the "What is Unbound Force?" landing page, the knowledge retrieval guide, or the projects section index encounters accurate descriptions of the hero lifecycle and Dewey's role. These pages reference the workflow model correctly and don't contradict the primary workflow documentation on the Common Workflows page.

**Why this priority**: These pages are secondary references that link to the primary workflow docs. They must be consistent but don't need the same level of detail. Inconsistency between pages is confusing and undermines trust.

**Independent Test**: Can be fully tested by reading each updated page and verifying that any mention of the hero lifecycle or Dewey's role is consistent with the Common Workflows and Product Owner pages.

**Acceptance Scenarios**:

1. **Given** a visitor on the "What is Unbound Force?" page, **When** they read the description of the swarm, **Then** the text reflects that the swarm can run autonomously from define through review (not just "from implementation through review").
2. **Given** a visitor on the knowledge retrieval guide, **When** they read about Dewey's role, **Then** the page mentions that Dewey enables autonomous define, not just context enhancement for a human-driven define stage.
3. **Given** a visitor on the projects section index, **When** they look at the current projects listing, **Then** Dewey appears alongside Gaze as a current project.

---

### User Story 5 - Homepage Dewey Visibility (Priority: P5)

A first-time visitor to the Unbound Force homepage sees that the organization has two major projects — Gaze and Dewey — not just one. The homepage projects section includes a Dewey project card, and the CTA buttons reflect both projects.

**Why this priority**: The homepage is the first impression, but this is a pre-existing gap from Phase 4 rather than a Phase 5 requirement. It should be fixed in the same PR since we're already editing the homepage layout. Lower priority because the docs pages are the primary deliverable for Phase 5.

**Independent Test**: Can be fully tested by loading the homepage and verifying that the projects section contains both a Gaze card and a Dewey card, and that at least one CTA references Dewey.

**Acceptance Scenarios**:

1. **Given** a visitor on the homepage, **When** they scroll to the Projects section, **Then** they see project cards for both Gaze and Dewey.
2. **Given** a visitor viewing the Dewey project card, **When** they click it, **Then** they are taken to the Dewey GitHub repository page.

---

### Edge Cases

- What happens when a visitor reads the workflow documentation before Phase 5 is implemented in the meta repo? The documentation should describe the target state (autonomous define as the default) since it will be published alongside or after the meta repo changes. The current 2-checkpoint model should be documented as a fallback/legacy mode, not the primary path.
- What happens when a visitor follows links between updated and non-updated pages? All cross-page references (links to Common Workflows, Product Owner guide, Dewey pages) must be verified to avoid dead links or contradictory descriptions.
- How does the documentation handle projects where Dewey is not installed? The autonomous define documentation must note that the 1-checkpoint model requires Dewey. Without Dewey, the 2-checkpoint model applies. This is consistent with Dewey's existing graceful degradation tiers.

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: The Common Workflows page MUST describe the define stage with a configurable execution mode — both `[human]` and `[swarm]` — with the swarm mode presented as the default for Dewey-enabled projects.
- **FR-002**: The Common Workflows page MUST document the "seed" interaction for swarm-mode define: the human provides 1-2 sentences of intent, and Muti-Mind creates the full specification using Dewey's semantic context.
- **FR-003**: The Common Workflows page MUST document the optional specification review checkpoint between the define and implement stages, including when to enable it and what to evaluate.
- **FR-004**: The Swarm Delegation section MUST describe both the 1-checkpoint model (seed + accept) and the 2-checkpoint model (define + accept), explaining when each applies.
- **FR-005**: The Product Owner guide MUST update "Your Role in the Hero Lifecycle" to describe the seed interaction as the default, with examples of effective seeds.
- **FR-006**: The Product Owner guide MUST document the optional spec review checkpoint with guidance on when to use it and what to look for.
- **FR-007**: The Dewey project page MUST mention autonomous define as a key capability enabled by Dewey's semantic context.
- **FR-008**: The Dewey team page MUST update Muti-Mind's entry in the "How Heroes Use Dewey" section to reflect autonomous specification drafting.
- **FR-009**: The "What is Unbound Force?" landing page MUST update its description of swarm autonomy to include the define stage.
- **FR-010**: The knowledge retrieval guide MUST mention Dewey's role in enabling autonomous define.
- **FR-011**: The projects section index MUST list Dewey alongside Gaze under "Current Projects."
- **FR-012**: The homepage MUST include a Dewey project card in the projects section alongside the existing Gaze card.
- **FR-013**: All updated pages MUST pass `npm run build` with zero errors.
- **FR-014**: All cross-page links in updated content MUST resolve to valid pages.
- **FR-015**: All content MUST be accurate per the Dewey design paper and orchestration plan — no fabricated capabilities or overstated maturity.
- **FR-016**: No page MUST contain placeholder or "coming soon" content per the zero-waste mandate.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: A reader of the Common Workflows page can identify both the 1-checkpoint and 2-checkpoint workflow models and understand which applies to their project within 2 minutes of reading.
- **SC-002**: A Product Owner reading their guide understands their new role (seed + accept) and can write an effective seed after reading the examples, without needing to consult other documentation.
- **SC-003**: 100% of cross-page links in updated content resolve to valid pages (verified by `npm run build` and manual link check).
- **SC-004**: The Dewey project page and team page both mention autonomous define as a Dewey-enabled capability — connecting features (semantic search) to outcomes (reduced human checkpoints).
- **SC-005**: The homepage projects section displays both Gaze and Dewey project cards, each linking to their respective GitHub repositories.
- **SC-006**: All updated content is consistent — no page contradicts another regarding the workflow model, Dewey's role, or the number of human checkpoints.
- **SC-007**: `npm run build` succeeds with zero errors after all changes are applied.
- **SC-008**: Both light mode and dark mode render correctly for all updated pages (visual verification via `npm run dev`).

## Assumptions

- Phase 5 meta repo changes (configurable execution modes, Muti-Mind SKILL.md updates) will be implemented before or concurrently with this website work. The website documents the target state.
- The 1-checkpoint model (seed + accept) is presented as the default for projects with Dewey configured. The 2-checkpoint model (define + accept) is presented as the fallback for projects without Dewey.
- The optional specification review checkpoint is a lightweight gate — the PO scans the spec for intent alignment, not a full spec editing session.
- The existing Dewey project page (73 lines) and team page (129 lines) are expanded with Phase 5 content, not rewritten from scratch.
- Homepage changes (Dewey project card) follow the existing Gaze card pattern — same card structure, linking to the GitHub repository.
