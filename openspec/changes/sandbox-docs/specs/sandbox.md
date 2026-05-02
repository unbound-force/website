## ADDED Requirements

### Requirement: sandbox-docs-page

The website MUST have a sandbox documentation page covering all `uf sandbox` subcommands, mount modes, and platform prerequisites.

#### Scenario: user visits sandbox docs

- **GIVEN** a user navigates to the sandbox documentation page
- **WHEN** the page renders
- **THEN** all 7 subcommands MUST be documented: `start`, `stop`, `attach`, `extract`, `status`, `create`, `destroy`
- **AND** both mount modes (isolated and direct) MUST be explained
- **AND** API key forwarding behavior MUST be documented

### Requirement: sandbox-uid-mapping-docs

The sandbox page MUST include a UID mapping section covering prerequisites, platform-specific setup, and the `--uidmap` escape hatch.

#### Scenario: macOS user sets up sandbox

- **GIVEN** a macOS user reads the sandbox docs
- **WHEN** they reach the UID mapping section
- **THEN** macOS Podman machine configuration steps MUST be documented
- **AND** the `--uidmap` flag MUST be documented as an escape hatch
- **AND** the Podman >= 4.3 version requirement MUST be stated

#### Scenario: user encounters root file ownership

- **GIVEN** a user sees files appearing as `root:nobody` inside the sandbox
- **WHEN** they look for troubleshooting help
- **THEN** the UID mapping section MUST explain the cause and resolution

### Requirement: sandbox-persistent-workspace-docs

The sandbox page MUST include a persistent workspaces section covering `create`/`destroy` commands, named volumes, and workspace lifecycle.

#### Scenario: user creates persistent workspace

- **GIVEN** a user reads the persistent workspaces section
- **WHEN** they follow the documentation
- **THEN** `uf sandbox create` flags (`--backend`, `--demo-ports`, `--name`) MUST be documented
- **AND** `uf sandbox destroy` flags (`--yes`, `--force`) and confirmation prompt MUST be documented
- **AND** workspace detection behavior (existing workspaces used by `start`/`stop`/`extract`) MUST be explained

### Requirement: sandbox-cde-backend-docs

The sandbox page MUST include a CDE backend section labeled as experimental.

#### Scenario: user reads CDE backend section

- **GIVEN** a user reads the CDE backend section
- **WHEN** they see the experimental label
- **THEN** the section MUST clearly state the Che backend has not been validated end-to-end with production Eclipse Che
- **AND** the `--backend` flag and backend resolution matrix (flag > env var > config > auto-detect) MUST be documented

### Requirement: sandbox-security-model

The sandbox page MUST include a security model section as a table listing isolation properties.

#### Scenario: user evaluates sandbox security

- **GIVEN** a user reads the security model section
- **WHEN** they review the isolation properties
- **THEN** the table MUST include: rootless Podman, read-only mounts (isolated mode), no push credentials, resource limits, SELinux, non-root user

## MODIFIED Requirements

_None._

## REMOVED Requirements

_None._
