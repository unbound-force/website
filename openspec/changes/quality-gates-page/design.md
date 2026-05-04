## Context

Quality information is scattered across 5 pages. Issue #94 calls for a consolidated quality gates page. This complements the architecture page (#88) which covers the control matrix conceptually.

## Goals / Non-Goals

### Goals
- Present the 4-layer feedback stack in order (CI → Gaze → Divisor Council → Constitution Check)
- Explain computational-first, inferential-second ordering
- Cover gatekeeping value protection, doer/judge separation, 3-iteration cap
- Link to existing pages for deeper detail on individual components

### Non-Goals
- Duplicating content from the-divisor.md or tester.md
- Adding custom HTML or styling
- Modifying existing pages

## Decisions

**File**: `content/docs/getting-started/quality-gates.md` with weight 83.

**Structure**: The pipeline overview → Layer 1 (CI) → Layer 2 (Gaze) → Layer 3 (Divisor Council) → Layer 4 (Constitution Check) → Gatekeeping protection → CTA.

**Cross-references**: Link to tester.md, the-divisor.md, constitution.md, and common-workflows.md for deeper detail.

## Risks / Trade-offs

**Risk**: Overlap with common-workflows.md code review section. Mitigation: this page focuses on the quality architecture (why the layers exist and how they compose), while common-workflows covers the operational sequence (when each step runs during /unleash).
