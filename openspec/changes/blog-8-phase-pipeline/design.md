## Context

The unleash-in-practice post covers the pipeline mechanics. Issue #97 calls for a post explaining why 8 phases exist instead of the 2 that industry consensus recommends.

## Goals / Non-Goals

### Goals
- Explain why two-phase plan/execute is insufficient for complex features
- Walk through each of the 8 phases and why each gate exists
- Cover the enforcement mechanisms (filesystem markers, branch naming, spec commit gate, CI parity gate)
- Show how the pipeline enforces incrementalism at the specification level

### Non-Goals
- Duplicating the step-by-step operational guide from unleash-in-practice
- Adding custom HTML or styling
- Modifying existing pages

## Decisions

**File**: `content/blog/the-8-phase-pipeline.md` with slug `8-phase-pipeline`.

**Structure**: The two-phase problem → Why 8 phases → Walking through each gate → The enforcement mechanisms → How this differs from industry standard → CTA.

## Risks / Trade-offs

Overlap with unleash-in-practice post. Mitigation: this post is "why" (architectural rationale), that post is "how" (operational mechanics). Different questions, complementary content.
