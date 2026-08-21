## ADDED Requirements

### Requirement: uf-setup-lifecycle-section

The Common Workflows page (`content/docs/getting-started/common-workflows.md`) MUST include a "uf setup / uf init Lifecycle" section that documents the full initialization lifecycle.

The section MUST cover:
1. What `uf setup` installs and in what order (install cascade)
2. Platform detection behavior (Homebrew, dnf, curl fallback)
3. The `--dry-run` flag for previewing changes
4. What `uf init` scaffolds (the 12-step process at a conceptual level)
5. Sub-tool initialization (concurrent execution of Dewey, Gaze, Replicator, Specify, OpenSpec)
6. Group A vs Group B sub-tool classification
7. The `--force` flag behavior for re-initialization across both groups
8. Command-specific guardrail injection and self-correction on re-runs
9. Stale command reference detection and warnings after upgrades
10. A troubleshooting subsection covering common issues

The section SHOULD be placed after the existing "Environment Setup" section and before "Next Steps".

#### Scenario: User reads lifecycle section after running uf setup
- **GIVEN** a user has run `uf setup` and wants to understand what happened
- **WHEN** they navigate to the Common Workflows page and scroll to the lifecycle section
- **THEN** they find an organized explanation of the setup install cascade, init scaffolding steps, sub-tool initialization, and `--force` re-initialization behavior

#### Scenario: User encounters Dewey hang during uf init --force
- **GIVEN** a user runs `uf init --force` and Dewey indexing appears to hang
- **WHEN** they check the troubleshooting subsection
- **THEN** they find an explanation that `--force` passes `--no-embeddings` to Dewey and guidance to run `dewey index` separately if full embedding generation is needed

#### Scenario: User sees stale command reference warnings
- **GIVEN** a user upgrades `uf` and runs `uf init` on an existing project
- **WHEN** they see warnings about stale command references to pre-`uf.*` namespace commands
- **THEN** they find in the troubleshooting subsection an explanation that init detects and warns about old-name commands, with guidance to update agent markdown files

### Requirement: uf-setup-rpm-resolution-docs

The lifecycle section MUST document that on Fedora/RHEL (dnf-based) systems, `uf setup` resolves companion tool RPM versions independently via `gh release view` rather than using the `uf` binary's own version.

The documentation SHOULD note that `gh` CLI must be installed before Gaze and Replicator on dnf-based systems.

#### Scenario: Fedora user encounters RPM 404 errors
- **GIVEN** a Fedora user runs `uf setup` and gets 404 errors when downloading companion tool RPMs
- **WHEN** they check the troubleshooting subsection
- **THEN** they find an explanation that RPM versions are resolved independently per tool, and guidance to verify `gh` CLI is installed and has network access

### Requirement: quick-start-cross-reference

The Quick Start page (`content/docs/getting-started/quick-start.md`) MUST include an inline cross-reference link to the lifecycle section for users who want to understand the setup process in more depth.

#### Scenario: Quick Start reader wants more detail
- **GIVEN** a user is reading the Quick Start page
- **WHEN** they reach the `uf setup` section
- **THEN** they find a link to the Common Workflows lifecycle section for deeper understanding

### Requirement: developer-guide-cross-reference

The Developer Guide page (`content/docs/getting-started/developer.md`) MUST include an inline cross-reference link from the Prerequisites section to the lifecycle section.

#### Scenario: Developer Guide reader wants init details
- **GIVEN** a user is reading the Developer Guide Prerequisites section
- **WHEN** they see the `uf setup` command
- **THEN** they find a link to the Common Workflows lifecycle section for the full initialization details

## MODIFIED Requirements

_None._

## REMOVED Requirements

_None._
