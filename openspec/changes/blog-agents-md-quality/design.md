## Context

The website documents `/agent-brief` in `common-workflows.md` (PR #74) with create/audit modes, 5-tier rubric, and bridge file management. Issue #65 provides a narrative angle emphasizing broad appeal — AGENTS.md quality matters for any AI coding agent, not just Unbound Force users.

## Goals / Non-Goals

### Goals
- Write a blog post following the BA-001 narrative arc
- Lead with "garbage in, garbage out" per BA-007 — frame from the universal problem of AI agent context quality
- Include the 5-tier scoring rubric as a self-assessment tool
- Explain the section taxonomy (Tier 1 / Tier 1C / Tier 2) so readers understand what makes a complete AGENTS.md
- Cover bridge files (CLAUDE.md, .cursorrules) for cross-tool reach
- Frame broadly — useful for Claude Code, Cursor, Copilot users, not just Unbound Force

### Non-Goals
- Duplicate the full command reference (flags, doctor integration) — link to common-workflows docs
- Tutorial-style AGENTS.md writing guide (different content type)
- Comparison with other context file formats (e.g., Cursor rules files)

## Decisions

1. **Slug and filename**: `agents-md-quality` — emphasizes the quality dimension. File at `content/blog/agents-md-quality.md`.

2. **Universal opening**: Start with a problem any developer using AI agents faces — not tool-specific. Mention Claude Code, Cursor, Copilot by name to signal broad relevance and improve search discoverability.

3. **Rubric as centerpiece**: The 5-tier scoring rubric is the key takeaway — readers should be able to self-assess their own AGENTS.md after reading the post.

4. **Section taxonomy as education**: Explain what Tier 1 / Tier 1C / Tier 2 sections are. This teaches the reader what a complete AGENTS.md looks like, independent of any specific tool.

5. **Frontmatter pattern**: Follow existing blog post frontmatter.

6. **Cross-references**: Link to common-workflows docs where `/agent-brief` is documented (PR #74, if merged).

## Content Sources

Authoritative upstream source:
- `/agent-brief` command: `unbound-force/unbound-force/.opencode/command/agent-brief.md`

All technical claims must be verified against the upstream command file at implementation time.

## Risks / Trade-offs

- **Risk**: The "broad appeal" framing could make the post feel generic. Mitigated by including specific `/agent-brief` examples alongside the universal principles.
- **Trade-off**: Depth vs. breadth. Chose breadth — the post teaches the concept of AGENTS.md quality and introduces `/agent-brief` as one implementation. Deep `/agent-brief` usage is covered in the reference docs.
