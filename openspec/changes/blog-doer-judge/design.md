## Context

The Divisor Council enforces doer/judge separation structurally — the Guard agent cannot modify files. No blog post explains this rationale. Issue #91 calls for a post covering structural vs behavioral enforcement.

## Goals / Non-Goals

### Goals
- Explain why structural enforcement (tool access restrictions) is superior to behavioral instructions
- Describe the layered feedback stack (CI → Gaze → Divisor agents)
- Cover the 3-iteration review cap as a pragmatic constraint
- Attribute Anthropic findings to Liu article

### Non-Goals
- Detailed Divisor agent persona documentation (that belongs in team pages)
- Modifying existing pages
- Adding diagrams or custom HTML

## Decisions

**File**: `content/blog/why-your-ai-code-reviewer-cannot-have-write-access.md` with slug `ai-code-reviewer-write-access`.

**Structure**: Problem (collapsed doer/judge) → Structural vs behavioral → The layered feedback stack → The 3-iteration cap → CTA.

**Cross-references**: Link to the-divisor.md and gaze-tester.md team pages; common-workflows.md for the review pipeline.

## Risks / Trade-offs

**Risk**: Overlap with the five-principles post (#89) which also covers doer/judge separation. Mitigation: this post goes deeper on the specific mechanism (tool access restrictions, review iteration limits) while the principles post covers it at the conceptual level.
