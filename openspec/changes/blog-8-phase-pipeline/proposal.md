## Why

Industry consensus says plan and execute must be separate. But "plan then execute" is only two phases. For complex features, you need gates between clarification, planning, task generation, spec review, implementation, code review, and retrospective. The existing unleash-in-practice post covers the pipeline mechanics; this post explains the architectural rationale for 8 phases instead of 2.

GitHub Issue: #97

## What Changes

- Add a new blog post at `content/blog/the-8-phase-pipeline.md`

## Capabilities

### New Capabilities
- `blog-8-phase-pipeline`: Blog post explaining why plan/execute separation extends to 8 phases with hard gates

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- New file: `content/blog/the-8-phase-pipeline.md`
- No changes to existing pages

## Constitution Alignment

Assessed against the **Unbound Force Website** constitution (.specify/memory/constitution.md).

### I. Content Accuracy

**Assessment**: PASS

Pipeline phases, exit points, and enforcement mechanisms are verifiable against the /unleash command and AGENTS.md.

### II. Minimal Footprint

**Assessment**: PASS

Pure Markdown blog post.

### III. Visitor Clarity

**Assessment**: PASS

Clear narrative explaining the architectural rationale with concrete evidence.
