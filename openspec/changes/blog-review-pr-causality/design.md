## Context

The website documents `/review-pr` in `common-workflows.md` (PR #74) with command reference, CI causality table, fix branch offering, and comparison table with `/review-council`. Issue #64 provides a narrative angle, key messages, and source references for a blog post that explains the "why" behind CI causality analysis as a broadly useful pattern.

## Goals / Non-Goals

### Goals
- Write a blog post following the BA-001 narrative arc (problem → approach → evidence → CTA)
- Lead with "not your fault? not your problem" per BA-007 — frame from the developer's CI triage frustration
- Include the causality classification table (base pass/fail × PR pass/fail → classification)
- Cover the fix branch offering as the "fix it forward" pattern
- Position `/review-council` + `/review-pr` as a complete review lifecycle
- Include the in-line PR comments feature with 15-comment cap

### Non-Goals
- Duplicate the full command reference (flags, phases) — link to common-workflows docs
- Deep dive into the AI judgment phase — focus on the CI causality pattern
- Performance analysis of the reliability improvements from the follow-up change
- Tutorial-style step-by-step guide (tracked in issue #66)

## Decisions

1. **Slug and filename**: `review-pr-causality` — emphasizes the causality analysis which is the unique value proposition. File at `content/blog/review-pr-causality.md`.

2. **Universal framing**: Open with a problem every developer faces (CI failures on PRs) before introducing `/review-pr`. The causality analysis pattern is useful regardless of whether the reader uses Unbound Force.

3. **Causality table as centerpiece**: The 2×2 classification table (base pass/fail × PR pass/fail) is the key insight. Make it a prominent, scannable element.

4. **Review lifecycle diagram**: Show the two commands as a timeline: develop → `/review-council` → push → create PR → `/review-pr`. This positions them as complementary rather than competing.

5. **Frontmatter pattern**: Follow existing blog post frontmatter.

6. **Cross-references**: Link to common-workflows docs where `/review-pr` is fully documented (PR #74, if merged). Link to getting-started pages which always exist.

## Content Sources

Authoritative upstream source:
- `/review-pr` command: `unbound-force/unbound-force/.opencode/command/review-pr.md`
- Comparison table: `content/docs/getting-started/common-workflows.md` (if PR #74 merged)

All technical claims must be verified against the upstream command file at implementation time.

## Risks / Trade-offs

- **Risk**: The "fix branch" feature has specific behavioral guards (dirty-tree guard, collision check) that are easy to describe inaccurately. Mitigated by verification task against upstream source.
- **Trade-off**: Universal framing vs. tool-specific depth. Chose universal — the CI causality pattern is the hook, `/review-pr` is the implementation. Readers who don't use Unbound Force still get value from understanding the pattern.
