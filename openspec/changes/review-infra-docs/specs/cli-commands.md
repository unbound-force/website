## ADDED Requirements

### Requirement: OpenCode Slash Commands Section

The CLI reference page (`content/docs/reference/cli.md`) MUST be updated to include a new top-level section for OpenCode slash commands, clearly separated from the existing `uf` CLI subcommands. The section SHOULD be introduced with a paragraph explaining that slash commands are typed within an AI coding session (e.g., OpenCode), not in a terminal. Each command section below MUST follow the existing CLI reference pattern (usage code block, description, flags/options table where applicable).

### Requirement: Agent Brief Command Reference

The CLI reference page (`content/docs/reference/cli.md`) MUST be updated to include documentation for the `/agent-brief` command under the new OpenCode Slash Commands section. The section MUST cover:

- Command purpose: AGENTS.md lifecycle management
- Two modes: `create` (generate AGENTS.md from project analysis) and `audit` (validate existing AGENTS.md for completeness and accuracy)
- Usage syntax and options
- Example output for each mode

#### Scenario: User looks up /agent-brief usage

- **GIVEN** a user wants to generate or audit their project's AGENTS.md
- **WHEN** they read the `/agent-brief` section in the CLI reference
- **THEN** they understand the two modes (create/audit), their purposes, and how to invoke them

### Requirement: Review PR Command Reference

The CLI reference page (`content/docs/reference/cli.md`) MUST be updated to include documentation for the `/review-pr` command under the OpenCode Slash Commands section. The section MUST cover:

- Command purpose: post-PR GitHub review with CI causality analysis
- How it differs from `/review-council` (post-PR vs pre-PR)
- CI causality analysis: distinguishing pre-existing CI failures from PR-introduced failures
- Usage syntax and options
- Cross-reference to the code review tutorial for workflow context

The section SHOULD note that the code-review-tutorial.md provides a hands-on walkthrough of `/review-pr` in practice.

#### Scenario: User looks up /review-pr usage

- **GIVEN** a user has submitted a PR and wants AI review with CI analysis
- **WHEN** they read the `/review-pr` section in the CLI reference
- **THEN** they understand the command's purpose, CI causality analysis capability, and how to invoke it

### Requirement: Address Feedback Command Reference

The CLI reference page (`content/docs/reference/cli.md`) MUST be updated to include documentation for the `/address-feedback` command under the OpenCode Slash Commands section. The section MUST cover:

- Command purpose: PR review feedback triage and resolution
- The four-phase workflow (collect, classify, plan, execute)
- How it integrates with the review lifecycle (used after receiving review comments)
- Usage syntax and options

#### Scenario: User looks up /address-feedback usage

- **GIVEN** a user has received PR review comments and wants to systematically address them
- **WHEN** they read the `/address-feedback` section in the CLI reference
- **THEN** they understand the four-phase workflow and how to invoke the command

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
