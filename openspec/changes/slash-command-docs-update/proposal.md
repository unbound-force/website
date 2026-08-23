## Why

The Unbound Force toolchain has undergone a significant slash command namespace migration — all hero-specific commands moved from flat names (`/unleash`, `/finale`, `/review-council`) to the `uf.*` namespace (`/uf.unleash`, `/uf.finale`, `/uf.review-council`). Additionally, several commands gained new capabilities: `/uf.review-council` now uses soft-gate CI causality analysis, `/uf.review-pr` posts verdicts for all review outcomes, `/uf.finale` supports sub-agent merge conflict resolution and structured PR descriptions with AI attribution, and a new `/uf.triage-issue` command was added for multi-agent issue triage.

The website currently uses the old command names exclusively across 14+ content pages, 1 layout template, blog posts, and internal documentation. Every reference is stale. Visitors following the documentation will encounter command names that no longer exist in the shipped toolchain.

This change updates all website documentation to reflect the current state of the shipped commands, covering 7 tracked issues: #220, #221, #224, #179, #162, #204, #202.

## What Changes

1. **Namespace migration across all content** (#220): Replace every old command name with its `uf.*` equivalent across all pages, blog posts, templates, and configuration files. Add documentation for new commands (`/uf.triage-issue`) and new non-namespaced commands (`/forge`, `/forge:status`, `/org`, `/inbox`, `/handoff`).

2. **Review council soft-gate documentation** (#221): Update all `/uf.review-council` documentation to reflect the soft-gate CI causality analysis behavior — pre-existing CI failures on `main` no longer block reviews. Document the two-tier baseline strategy (GitHub CI API check first, git worktree fallback).

3. **Review PR verdict posting** (#224): Update `/uf.review-pr` documentation to reflect that verdict posting now appears for all review outcomes regardless of finding severity, not just for CRITICAL/HIGH findings.

4. **GitHub review posting** (#179): Document the ability to post review council findings as GitHub PR reviews, including PR detection, multi-persona aggregation, verdict mapping (APPROVE/REQUEST_CHANGES/COMMENT), and the human confirmation requirement.

5. **Finale structured PR descriptions** (#162): Document the structured PR body format (Summary, How to Test, How to Demo, Key Files Changed), AI attribution footer and git trailer, PR template detection, and review-council findings integration.

6. **Finale sub-agent merge conflict resolution** (#204): Document the new Option 5 in conflict recovery — spawning a sub-agent for AI-assisted merge conflict resolution.

7. **Triage-issue command** (#202): Add documentation for the new `/uf.triage-issue` command — multi-agent issue triage using 5 Divisor agents with 7 classification categories and structured output.

## Capabilities

### New Capabilities
- `triage-issue-docs`: Documentation for `/uf.triage-issue` multi-agent issue triage command
- `forge-command-docs`: Documentation for `/forge`, `/forge:status` commands
- `session-command-docs`: Documentation for `/org`, `/inbox`, `/handoff` session management commands
- `github-review-posting-docs`: Documentation for posting review council findings as GitHub PR reviews
- `conflict-sub-agent-docs`: Documentation for AI-assisted merge conflict resolution in `/uf.finale`
- `structured-pr-docs`: Documentation for structured PR descriptions and AI attribution

### Modified Capabilities
- `command-references`: All old command names (`/unleash`, `/finale`, `/cobalt-crush`, `/review-council`, `/constitution-check`, `/uf-init`) replaced with `uf.*` namespace equivalents
- `review-council-docs`: Updated to reflect soft-gate CI causality behavior and GitHub review posting
- `review-pr-docs`: Updated to reflect verdict posting for all outcomes
- `finale-docs`: Updated to include structured PR bodies, AI attribution, and sub-agent conflict resolution
- `code-review-tutorial`: Updated with new command names and new capabilities

### Removed Capabilities
- None — all old capabilities are preserved under new names

## Impact

### Files requiring changes (identified via grep)

**Documentation pages** (primary content):
- `content/docs/getting-started/common-workflows.md` — heaviest impact; `/unleash`, `/finale`, `/cobalt-crush`, `/review-council` references throughout
- `content/docs/getting-started/developer.md` — `/unleash`, `/finale`, `/cobalt-crush` references in workflows and examples
- `content/docs/getting-started/quick-start.md` — `/unleash`, `/finale`, `/cobalt-crush` in code examples
- `content/docs/getting-started/code-review-tutorial.md` — `/review-council`, `/review-pr` throughout
- `content/docs/getting-started/tester.md` — `/review-council` reference
- `content/docs/getting-started/product-owner.md` — `/unleash` reference
- `content/docs/getting-started/architecture.md` — `/unleash`, `/review-council`, `/cobalt-crush` references
- `content/docs/getting-started/_index.md` — `/cobalt-crush`, `/unleash` references
- `content/docs/getting-started/constitution.md` — `/unleash`, `/review-council`, `/constitution-check` references

**Team pages** (reviewed — no command references, persona names only):
- `content/docs/team/_index.md` — "Cobalt-Crush" persona name only, no slash command invocations
- `content/docs/team/cobalt-crush.md` — persona page, no slash command invocations

**Blog posts**:
- `content/blog/unleash-in-practice.md` — heavy `/unleash` and `/finale` usage throughout
- `content/blog/dewey-vs-karpathy.md` — `/unleash` reference
- `content/blog/five-principles-every-ai-agent-harness-discovers.md` — `/unleash` reference
- `content/blog/sandbox-isolation.md` — `/unleash` reference
- `content/blog/the-8-phase-pipeline.md` — `/unleash` reference

**Layout templates**:
- `layouts/home.html` — `/unleash` in hero section code reference

**SCSS**:
- `assets/scss/common/_custom.scss` — comment referencing `/unleash` card

**Internal docs** (AGENTS.md):
- `AGENTS.md` — `/review-council` references in Review Council section

### Key distinction: command name vs persona name

"Cobalt-Crush" is both a persona name (the team member identity) and a command name (`/cobalt-crush` → `/uf.cobalt-crush`). The persona name stays unchanged on team pages; only the slash command invocation updates. Same applies to other team pages — the persona names are not changing, only the `/command` syntax.

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

This change updates documentation content only. No agent artifacts, communication protocols, or artifact formats are affected. Agents continue to collaborate through the same well-defined artifacts — the documentation simply reflects the current command names accurately.

### II. Composability First

**Assessment**: N/A

Documentation changes do not affect agent installability or standalone functionality. The commands being documented maintain their composable nature — this change ensures the documentation accurately reflects that composability.

### III. Observable Quality

**Assessment**: PASS

This change improves observable quality by ensuring documentation matches shipped behavior. Visitors reading the docs will get accurate command names and accurate descriptions of capabilities (soft-gate CI analysis, structured PRs, verdict posting), reducing confusion and failed attempts to use non-existent command names.

### IV. Testability

**Assessment**: N/A

No code, tests, or testable components are affected. The website has no test suite — validation is manual via `npm run build` and visual verification, which will be performed as part of the implementation CI parity gate.

### V. Security by Default

**Assessment**: N/A

Documentation-only changes with no security implications. No dependencies added, no external inputs processed, no permissions changed.
