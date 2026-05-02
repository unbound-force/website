## ADDED Requirements

_None._

## MODIFIED Requirements

### Requirement: finale-description-accuracy

The website MUST describe `/finale` as creating a PR for review. The website MUST NOT state that `/finale` merges PRs.

Previously: `/finale` was described as merging PRs with rebase strategy, including a "Merge PR" step in the workflow table.

#### Scenario: user reads finale description in common-workflows

- **GIVEN** a user visits the Common Workflows page
- **WHEN** they read the `/finale` workflow steps
- **THEN** the steps MUST NOT include a "Merge PR" step
- **AND** the guardrails MUST state "Never merges the PR — creates PRs for review, not for immediate merge"
- **AND** the description MUST say "/unleash builds, /finale wraps up the branch and creates a PR for review"

#### Scenario: user reads finale in quick-start

- **GIVEN** a user visits the Quick Start page
- **WHEN** they read the workflow code block
- **THEN** the `/finale` comment MUST say "commit, push, create PR" not "ship it"

#### Scenario: user reads finale in developer guide

- **GIVEN** a user visits the Developer Guide page
- **WHEN** they read the daily workflow section
- **THEN** the text MUST say "run `/finale` to commit, push, and create a PR"
- **AND** MUST NOT say "and merge"

### Requirement: unleash-branch-support-accuracy

The website MUST describe `/unleash` as supporting both `NNN-*` and `opsx/*` branches. The website MUST NOT state that `/unleash` rejects `opsx/*` branches.

Previously: `/unleash` was described as rejecting `opsx/*` branches.

#### Scenario: user reads unleash description

- **GIVEN** a user visits the Common Workflows page
- **WHEN** they read the `/unleash` section
- **THEN** the text MUST state that `/unleash` works with both Speckit (`NNN-*`) and OpenSpec (`opsx/*`) branches
- **AND** MUST explain that for OpenSpec branches, it detects the change name and reads tasks from `openspec/changes/<name>/tasks.md`

### Requirement: uf-init-file-count-accuracy

The website MUST state the correct approximate file count for `uf init` scaffolding and MUST list accurate command and agent counts.

Previously: File count stated as "approximately 50 files" with stale breakdown (Commands ~15, Agents ~12 with incomplete persona list).

#### Scenario: user reads uf init scaffolding details

- **GIVEN** a user visits the Developer Guide page
- **WHEN** they read the `uf init` section
- **THEN** the file count MUST state "approximately 35 files"
- **AND** the Commands count MUST be 8 with the correct list (review-council, review-pr, constitution-check, cobalt-crush, unleash, uf-init, agent-brief, finale)
- **AND** the Agents count MUST list 9 Divisor personas (6 review + 3 content), Cobalt-Crush, Constitution Check, Mx F Coach

### Requirement: mxf-install-accuracy

The website MUST NOT imply that `mxf` is bundled with the `unbound-force` Homebrew package. The install instruction MUST show the separate formula or the `uf setup` path.

Previously: Install instruction stated `brew install unbound-force/tap/unbound-force` includes `mxf`.

#### Scenario: user reads mxf install instruction

- **GIVEN** a user visits the Product Manager guide
- **WHEN** they read the Mx F installation section
- **THEN** the instruction MUST show `brew install unbound-force/tap/mxf` as the direct install
- **AND** MAY show `uf setup` as an alternative that installs the full toolchain

## REMOVED Requirements

_None._
