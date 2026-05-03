## Why

AGENTS.md is the first file every AI coding agent reads. A weak or missing AGENTS.md means the agent starts with wrong assumptions about conventions, build commands, and project structure — leading to code that compiles but does not fit. This is a "garbage in, garbage out" problem that affects every team using AI coding agents, regardless of which agent they use (Claude Code, Cursor, OpenCode, Copilot).

`/agent-brief` creates AGENTS.md from project signals or audits existing ones with a 5-tier scoring rubric. The command is documented in `common-workflows.md` (PR #74), but there is no narrative content explaining why AGENTS.md quality matters and how to improve it. This topic has broad search appeal — "AGENTS.md best practices" and "AI agent context files" are queries that reach beyond Unbound Force users.

This change addresses [GitHub issue #65](https://github.com/unbound-force/website/issues/65).

## What Changes

A new blog post at `content/blog/agents-md-quality.md` with the narrative arc:

1. **The problem** — AGENTS.md as the prompt that shapes all agent output; weak context → weak code
2. **What makes a good AGENTS.md** — the section taxonomy: Tier 1 (essential), Tier 1C (context-sensitive), Tier 2 (advanced)
3. **Audit, don't guess** — `/agent-brief audit` with 5-tier scoring rubric and 12 structural checks
4. **Create from signals** — `/agent-brief create` analyzes go.mod, Makefile, CI config, README
5. **Bridge files** — CLAUDE.md, .cursorrules, and cross-tool compatibility
6. **Broad applicability** — works for any AI coding agent, not just OpenCode

## Capabilities

### New Capabilities
- `blog/agents-md-quality`: Blog post on AGENTS.md quality and the `/agent-brief` command

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- 1 new blog post (Markdown content file)
- No layout, style, or configuration changes
- Cross-references to common-workflows docs and getting-started pages

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

All technical claims (5-tier rubric, 12 structural checks, section taxonomy, bridge file behavior) will be verified against the upstream implementation at `unbound-force/unbound-force/.opencode/command/agent-brief.md` at implementation time. The broad applicability claim (works for any AI agent) is accurate — AGENTS.md is a convention adopted across multiple tools.

### II. Minimal Footprint

**Assessment**: PASS

Single Markdown blog post file. No custom layouts, CSS, JavaScript, or dependencies.

### III. Visitor Clarity

**Assessment**: PASS

Follows the BA-001 narrative arc. Leads with a universal problem ("your AI agent's first read determines code quality") that applies to any team using AI coding agents. The 5-tier rubric gives visitors a concrete self-assessment tool.
