## ADDED Requirements

### Requirement: review-pr-docs

The website MUST document the `/review-pr` command in the Code Review section of `common-workflows.md`.

#### Scenario: user reads review-pr documentation

- **GIVEN** a user visits the Common Workflows page
- **WHEN** they read the Code Review section
- **THEN** `/review-pr` MUST be documented with: purpose, usage syntax (`/review-pr [N]`), auto-detection from current branch, CI causality analysis, fix branch offering
- **AND** a comparison table with `/review-council` MUST be present showing: when to use, scope, agent model

### Requirement: review-pr-causality-docs

The `/review-pr` documentation MUST explain CI causality analysis.

#### Scenario: user understands failure classification

- **GIVEN** a user reads the CI causality section
- **WHEN** they review how failures are classified
- **THEN** the difference between "PR-caused" and "pre-existing" failures MUST be explained
- **AND** the fix branch offering for pre-existing failures MUST be documented

### Requirement: review-pr-comparison-table

The Code Review section MUST include a comparison table showing when to use `/review-council` vs `/review-pr`.

#### Scenario: user decides which review command to use

- **GIVEN** a user wants to review code
- **WHEN** they read the comparison table
- **THEN** the table MUST cover at minimum: timing (pre-PR vs post-PR), scope, agent model, and use cases
- **AND** a decision guide MUST help users pick the right command for their situation

### Requirement: agent-brief-docs

The website MUST document the `/agent-brief` command.

#### Scenario: user reads agent-brief documentation

- **GIVEN** a user visits the relevant documentation page
- **WHEN** they read the `/agent-brief` section
- **THEN** both modes MUST be documented: create (generates AGENTS.md) and audit (scores existing AGENTS.md)
- **AND** the 5-tier scoring rubric (Excellent/Strong/Adequate/Weak/Missing) MUST be described
- **AND** context-sensitive section detection (constitution, spec framework) MUST be mentioned
- **AND** bridge file management (CLAUDE.md, .cursorrules) MUST be mentioned

### Requirement: quick-reference-table-update

The quick reference table in `common-workflows.md` MUST include entries for `/review-pr` and `/agent-brief`.

#### Scenario: user scans quick reference

- **GIVEN** a user scans the quick reference table
- **WHEN** they look for available commands
- **THEN** `/review-pr [N]` MUST be listed with brief description
- **AND** `/agent-brief` MUST be listed with brief description

## MODIFIED Requirements

_None._

## REMOVED Requirements

_None._
