## ADDED Requirements

### Requirement: cli-reference-page

The website MUST have a CLI reference page at `content/docs/reference/cli.md` that lists all current `uf` command groups and subcommands, sourced from `uf --help` output at implementation time.

Each command group MUST include:
- Command name and brief description
- Table of subcommands (if applicable) with flags and descriptions
- Link to detailed documentation page (if one exists)

The page MUST include a version marker indicating which `uf` version the reference was generated from (e.g., "This page reflects `uf` vX.Y.Z. Run `uf --help` for the latest."). Changelog entries MUST be derived from actual release artifacts (git tags, release notes, merged PRs).

#### Scenario: user visits CLI reference

- **GIVEN** a user navigates to the CLI reference page
- **WHEN** the page renders
- **THEN** all current command groups MUST be listed (expected at time of writing: `init`, `setup`, `doctor`, `config`, `sandbox`, `gateway`)
- **AND** each group MUST show its subcommands and primary flags
- **AND** the page MUST display a version marker indicating the `uf` version it was generated from

### Requirement: changelog-page

The website MUST have a changelog page at `content/docs/changelog/_index.md` tracking user-facing changes for the current release.

Each release entry MUST include:
- Version number and date
- Summary of user-facing changes grouped by type (Added, Changed, Fixed)

Changelog entries MUST be derived from actual release artifacts (git tags, release notes, merged PRs). Do not fabricate or estimate release contents.

#### Scenario: user checks what's new

- **GIVEN** a user navigates to the changelog page
- **WHEN** the page renders
- **THEN** releases MUST be listed in reverse chronological order (newest first)
- **AND** each release MUST summarize user-facing changes

### Requirement: reference-nav-section

The website navigation MUST include a "Reference" section in the docs sidebar containing the CLI reference page.

#### Scenario: user finds reference section

- **GIVEN** a user is browsing the documentation sidebar
- **WHEN** they look for command reference information
- **THEN** a "Reference" section MUST be visible in the navigation
- **AND** it MUST contain a link to the CLI reference page

## MODIFIED Requirements

### Requirement: stack-table-includes-cli

The stack positioning table on the getting-started index page (`content/docs/getting-started/_index.md`) and quick-start page (`content/docs/getting-started/quick-start.md`) MUST include the `uf` CLI as a layer.

Previously: The stack table listed 3 layers (Agent: OpenCode, Planning: Speckit, Coordination: Replicator). The heading "Three Tools, One Workflow" and surrounding prose MUST be updated to reflect the 4-layer model.

The updated table MUST show 4 layers:

| Layer | Tool | What It Does |
|-------|------|-------------|
| CLI | `uf` | Project scaffolding, environment setup, health checks, configuration, sandboxed execution, LLM gateway |
| Agent | OpenCode | AI coding environment |
| Planning | Speckit | Specification pipeline |
| Coordination | Replicator | Multi-agent coordination |

#### Scenario: user reads stack table on getting-started pages

- **GIVEN** a user visits the getting-started index or quick-start page
- **WHEN** they read the stack/tools table
- **THEN** the table MUST show 4 layers including the `uf` CLI layer
- **AND** the CLI layer MUST be positioned first (top of table)
- **AND** the section heading and prose MUST reflect the 4-layer model (not "Three Tools")

### Requirement: code-review-persona-scope

The Code Review section in `common-workflows.md` persona table MUST clarify that it covers review personas only, not content personas.

Previously: The table listed 6 personas without scoping the header.

#### Scenario: user reads persona table

- **GIVEN** a user reads the Code Review persona table
- **WHEN** they see the 6-persona list
- **THEN** a note MUST clarify that the 3 content personas (Scribe, Herald, Envoy) are invoked separately for content creation tasks and do not participate in code review

## REMOVED Requirements

_None._
