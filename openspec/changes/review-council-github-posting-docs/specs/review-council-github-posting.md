## ADDED Requirements

### Requirement: GitHub Review Posting Documentation in Code Review Tutorial

The Code Review Tutorial page (`content/docs/getting-started/code-review-tutorial.md`) MUST document the optional GitHub review posting capability as a subsection within Step 1.

The subsection MUST include:
1. The new command syntax: `/review-council [N]` and `/review-council code [N]` where `N` is an optional PR number
2. A description of what happens when a PR is detected or specified (findings are posted as a GitHub PR review)
3. The verdict mapping table showing how council verdicts map to GitHub review events
4. The pre-posting safety checks as a bulleted list
5. A note that human confirmation is required before posting
6. A note about graceful degradation when `gh` is unavailable or no PR exists

#### Scenario: Reader views updated Step 1 with GitHub posting subsection
- **GIVEN** a reader navigates to the Code Review Tutorial page
- **WHEN** they read Step 1 (Pre-PR Review with `/review-council`)
- **THEN** they see a subsection titled "Optional: Post to GitHub" that explains how to post council findings to a PR

#### Scenario: Reader understands verdict mapping
- **GIVEN** a reader is in the GitHub posting subsection
- **WHEN** they read the verdict mapping table
- **THEN** they see three rows mapping APPROVE → `APPROVE`, REQUEST CHANGES → `REQUEST_CHANGES`, and APPROVE WITH ADVISORIES → `COMMENT`

#### Scenario: Reader understands graceful degradation
- **GIVEN** a reader wants to use GitHub posting
- **WHEN** `gh` CLI is not installed or no PR exists
- **THEN** the documentation explains that `/review-council` falls back to full local-only behavior with no errors

### Requirement: Complete Loop Annotation

The "Complete Loop" section in the Code Review Tutorial MUST include a brief note after the ASCII workflow diagram acknowledging that `/review-council N` can optionally post findings to an existing PR. The diagram itself SHOULD NOT be modified — bare `/review-council` remains the primary workflow — but the explanatory paragraph must note the optional posting path.

#### Scenario: Reader understands optional post-PR usage in Complete Loop
- **GIVEN** a reader views the "Complete Loop" section
- **WHEN** they read the paragraph after the workflow diagram
- **THEN** they see a note that `/review-council N` can optionally post findings to an existing PR, complementing the standard pre-PR flow

### Requirement: APPROVE WITH ADVISORIES Verdict Explanation

The verdict mapping table in the Code Review Tutorial MUST include a brief explanation of the `APPROVE WITH ADVISORIES` verdict. This verdict occurs when all personas approve but one or more include advisory findings (LOW-severity observations or informational notes). It maps to GitHub's `COMMENT` event rather than `APPROVE` to ensure the advisory findings are visible on the PR without blocking merge.

#### Scenario: Reader understands when APPROVE WITH ADVISORIES occurs
- **GIVEN** a reader views the verdict mapping table
- **WHEN** they see the APPROVE WITH ADVISORIES → COMMENT row
- **THEN** there is a brief note explaining that this verdict occurs when all personas approve but include advisory findings

### Requirement: Updated Decision Table

The decision table in the Code Review Tutorial MUST include a row for the new "Post council findings to a PR" use case, distinguishing it from `/review-pr`.

#### Scenario: Reader decides between review-council with posting and review-pr
- **GIVEN** a reader is viewing the decision table
- **WHEN** they want to post review findings to a PR
- **THEN** they see a row explaining when to use `/review-council N` (multi-persona local review posted to PR) vs `/review-pr N` (CI-aware review with causality analysis)

### Requirement: Common Workflows Review Section Update

The Common Workflows page (`content/docs/getting-started/common-workflows.md`) MUST update the "Invoking the Council" subsection to document:
1. The optional PR number argument syntax
2. A brief note that findings can optionally be posted as a GitHub PR review when a PR number is provided

The "Verdict" subsection MUST also be updated to note that when GitHub posting is active, the council verdict maps to a GitHub review event (APPROVE → `APPROVE`, REQUEST CHANGES → `REQUEST_CHANGES`, APPROVE WITH ADVISORIES → `COMMENT`).

#### Scenario: Reader views updated invoking the council section
- **GIVEN** a reader navigates to the Common Workflows page
- **WHEN** they read the "Invoking the Council" subsection
- **THEN** they see both the basic `/review-council` invocation and the new `/review-council 42` syntax with a brief explanation

#### Scenario: Reader understands verdict-to-GitHub mapping in common workflows
- **GIVEN** a reader navigates to the Common Workflows page
- **WHEN** they read the "Verdict" subsection
- **THEN** they see a note explaining that when posting to GitHub, the council verdict maps to a GitHub review event

### Requirement: Developer Guide Command Reference Update

The Developer Guide page (`content/docs/getting-started/developer.md`) MUST update any reference to `/review-council` to note the optional PR number argument and GitHub posting capability.

#### Scenario: Reader views command reference
- **GIVEN** a reader navigates to the Developer Guide
- **WHEN** they see `/review-council` in a command list or reference
- **THEN** the description notes the optional GitHub review posting capability

## MODIFIED Requirements

### Requirement: /review-council Command Syntax

Previously: `/review-council` (no arguments)

The documentation MUST show the updated command syntax: `/review-council [N]` where `N` is an optional PR number. When `N` is provided, the council posts its findings as a GitHub PR review after local review completes.

### Requirement: /review-council vs /review-pr Comparison

Previously: `/review-council` is described as local-only pre-PR and `/review-pr` as post-PR with CI data.

The comparison MUST be updated to acknowledge that `/review-council` can now optionally post to GitHub PRs, while noting that `/review-pr` provides unique capabilities (CI causality analysis, PR metadata review) that `/review-council` does not.

## REMOVED Requirements

None — no existing documentation is removed, only updated.
