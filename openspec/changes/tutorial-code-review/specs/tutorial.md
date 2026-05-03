## ADDED Requirements

### Requirement: code-review-tutorial-page

The website MUST have a tutorial page at `content/docs/getting-started/code-review-tutorial.md` walking through the complete code review lifecycle.

#### Scenario: user follows code review tutorial

- **GIVEN** a user navigates to the code review tutorial
- **WHEN** the page renders
- **THEN** the tutorial MUST cover both `/review-council` (pre-PR) and `/review-pr` (post-PR) in sequence
- **AND** each step MUST include a command to run and expected output
- **AND** the tutorial MUST include prerequisites (uf init, gh CLI)

### Requirement: code-review-tutorial-decision-table

The tutorial MUST include a decision table showing when to use each review command.

#### Scenario: user decides which command to use

- **GIVEN** a user reads the decision table
- **WHEN** they evaluate their situation
- **THEN** the table MUST cover at least: before pushing, after creating PR, reviewing someone else's PR, and CI failure triage

### Requirement: code-review-tutorial-causality

The tutorial MUST include a CI causality analysis walkthrough.

#### Scenario: user understands causality classification

- **GIVEN** a user reads the causality section
- **WHEN** they see a CI failure on their PR
- **THEN** the tutorial MUST explain how `/review-pr` classifies failures as PR-caused vs pre-existing
- **AND** the tutorial MUST show the fix branch offering for pre-existing failures

### Requirement: code-review-tutorial-complete-loop

The tutorial MUST show how review commands fit into the complete development loop.

#### Scenario: user sees the full workflow

- **GIVEN** a user reads the complete loop section
- **WHEN** they understand the end-to-end flow
- **THEN** the tutorial MUST show: specify → unleash → /review-council → /finale → /review-pr

### Requirement: code-review-tutorial-frontmatter

The tutorial MUST use valid Hugo/Doks docs frontmatter consistent with existing pages.

#### Scenario: tutorial builds correctly

- **GIVEN** the tutorial file exists with frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build MUST succeed with no errors
- **AND** the page MUST appear in the Getting Started sidebar

## MODIFIED Requirements

_None._

## REMOVED Requirements

_None._
