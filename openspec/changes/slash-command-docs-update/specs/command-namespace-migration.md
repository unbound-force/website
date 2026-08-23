## ADDED Requirements

### Requirement: Namespace migration completeness

All slash command references in user-facing website content MUST use the `uf.*` namespace for hero-specific commands. The following mappings MUST be applied:

| Old Name | New Name |
|----------|----------|
| `/unleash` | `/uf.unleash` |
| `/finale` | `/uf.finale` |
| `/cobalt-crush` | `/uf.cobalt-crush` |
| `/constitution-check` | `/uf.constitution-check` |
| `/review-council` | `/uf.review-council` |
| `/uf-init` | `/uf.init` |
| `/review-pr` | `/uf.review-pr` |

Non-namespaced commands (`/forge`, `/forge:status`, `/org`, `/inbox`, `/handoff`) and Speckit commands (`/speckit.*`) are NOT affected by the namespace migration.

#### Scenario: Visitor reads documentation with current command names
- **GIVEN** a visitor reading any page under `content/docs/` or `content/blog/`
- **WHEN** they encounter a slash command reference for a hero-specific command
- **THEN** the command MUST use the `uf.*` namespace (e.g., `/uf.unleash` not `/unleash`)

#### Scenario: Persona names remain unchanged
- **GIVEN** a visitor reading a team page or a prose reference to an agent persona
- **WHEN** the text refers to the persona by name (e.g., "Cobalt-Crush", "the Divisor")
- **THEN** the persona name MUST remain unchanged — only slash command invocations update

#### Scenario: URL anchors preserved
- **GIVEN** an internal link referencing a heading-generated anchor (e.g., `#autonomous-pipeline-unleash`)
- **WHEN** the heading text is updated to use the new command name
- **THEN** the anchor MUST remain functional — either by preserving the original anchor text or using explicit Hugo anchor syntax

### Requirement: New command documentation

The website MUST document the following new commands in the Common Workflows page (`content/docs/getting-started/common-workflows.md`):

- `/uf.triage-issue` — multi-agent issue triage using 5 Divisor agents
- `/forge` and `/forge:status` — swarm coordination commands
- `/org` — work item management
- `/inbox` — message inbox
- `/handoff` — session handoff

#### Scenario: Visitor discovers new triage command
- **GIVEN** a visitor reading the Common Workflows page
- **WHEN** they scroll to the command reference sections
- **THEN** they MUST find documentation for `/uf.triage-issue` including its purpose (multi-agent issue evaluation), agent count (5 Divisor agents), classification categories, and the human-gated label mutation behavior

#### Scenario: Visitor discovers session management commands
- **GIVEN** a visitor reading the Common Workflows page
- **WHEN** they look for session management workflows
- **THEN** they MUST find documentation for `/org`, `/inbox`, and `/handoff` commands

### Requirement: Soft-gate CI causality documentation

The website MUST document that `/uf.review-council` uses soft-gate CI causality analysis. Pre-existing CI failures on `main` MUST be described as non-blocking informational findings.

#### Scenario: Visitor understands CI failure handling
- **GIVEN** a visitor reading `/uf.review-council` documentation
- **WHEN** they read about CI failure handling
- **THEN** the documentation MUST explain that pre-existing failures on `main` do not block the review verdict
- **AND** the two-tier baseline strategy (GitHub CI API check, git worktree fallback) SHOULD be mentioned

### Requirement: Verdict posting for all outcomes

The website MUST document that `/uf.review-pr` posts verdicts for all review outcomes, not just CRITICAL/HIGH findings.

#### Scenario: Visitor understands verdict posting
- **GIVEN** a visitor reading `/uf.review-pr` documentation
- **WHEN** they read about the review verdict step
- **THEN** the documentation MUST state that verdict posting occurs regardless of finding severity level

### Requirement: GitHub review posting

The website MUST document that `/uf.review-council` findings can be posted as GitHub PR reviews.

#### Scenario: Visitor understands GitHub review posting
- **GIVEN** a visitor reading review command documentation
- **WHEN** they read about post-review actions
- **THEN** the documentation MUST describe PR detection, verdict mapping (APPROVE/REQUEST_CHANGES/COMMENT), and the human confirmation requirement

### Requirement: Structured PR descriptions

The website MUST document that `/uf.finale` generates structured PR descriptions with the following sections: Summary, How to Test, How to Demo, Key Files Changed.

#### Scenario: Visitor understands PR description format
- **GIVEN** a visitor reading `/uf.finale` documentation
- **WHEN** they read about PR creation
- **THEN** the documentation MUST describe the structured PR body format and the AI attribution footer

### Requirement: Sub-agent merge conflict resolution

The website MUST document that `/uf.finale` supports AI-assisted merge conflict resolution via a spawned sub-agent.

#### Scenario: Visitor understands conflict resolution options
- **GIVEN** a visitor reading `/uf.finale` documentation
- **WHEN** they read about merge conflict handling
- **THEN** the documentation MUST describe the sub-agent conflict resolution option alongside existing manual options

## MODIFIED Requirements

### Requirement: AGENTS.md command references

AGENTS.md MUST use `/uf.review-council` instead of `/review-council` in the Review Council as PR Prerequisite section.

Previously: AGENTS.md referenced `/review-council` (old flat namespace).

#### Scenario: AGENTS.md uses current command names
- **GIVEN** an agent reading AGENTS.md for workflow guidance
- **WHEN** they encounter the Review Council section
- **THEN** the command reference MUST be `/uf.review-council`

### Requirement: Layout template command references

The homepage layout template (`layouts/home.html`) MUST reference `/uf.unleash` instead of `/unleash`.

Previously: The template used `/unleash` in the hero section.

#### Scenario: Homepage displays current command name
- **GIVEN** a visitor viewing the homepage
- **WHEN** they read the hero section or feature cards
- **THEN** the command reference MUST be `/uf.unleash`

### Requirement: Code review tutorial command references

The code review tutorial (`content/docs/getting-started/code-review-tutorial.md`) MUST use `/uf.review-council` and `/uf.review-pr` throughout.

Previously: The tutorial used `/review-council` and `/review-pr`.

## REMOVED Requirements

None — no existing requirements are being removed. All capabilities are preserved under new command names.
