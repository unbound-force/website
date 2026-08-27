## ADDED Requirements

### Requirement: DevPod failure diagnostic hint

The sandbox reference page MUST document that when `devpod up` fails, the CLI displays a diagnostic hint: `"run 'uf doctor' to diagnose or 'uf setup' to configure"`.

#### Scenario: User reads about DevPod workspace troubleshooting
- **GIVEN** a user is reading the sandbox reference page
- **WHEN** they look for information about DevPod workspace creation failures
- **THEN** they find documentation that `uf doctor` and `uf setup` are the recommended diagnostic and recovery commands

### Requirement: Changelog entry for pre-flight change

The changelog MUST include an entry documenting the removal of the standalone Podman binary pre-flight check for DevPod workspaces and the addition of the diagnostic hint.

#### Scenario: User checks changelog for breaking changes
- **GIVEN** a user is reading the changelog for the next release
- **WHEN** they look for sandbox-related changes
- **THEN** they find an entry describing the DevPod pre-flight change (removed `podman` binary check, added diagnostic hint)

## MODIFIED Requirements

### Requirement: Sandbox prerequisites

The sandbox reference prerequisites MUST distinguish between ephemeral containers (which require Podman installed) and DevPod persistent workspaces (which do not require a standalone `podman` binary in `$PATH`). The documentation MUST clarify that `uf setup` registers a docker-type DevPod provider under the name `podman` via `DOCKER_PATH=podman`, handling runtime resolution automatically.

Previously: "Prerequisites: Podman and DevPod must be installed."

#### Scenario: User reads prerequisites for DevPod workspace
- **GIVEN** a user wants to create a persistent DevPod workspace
- **WHEN** they read the sandbox prerequisites
- **THEN** they understand that `uf setup` configures the DevPod provider and they do not need to install standalone Podman separately

#### Scenario: User reads prerequisites for ephemeral sandbox
- **GIVEN** a user wants to run an ephemeral sandbox container
- **WHEN** they read the sandbox prerequisites
- **THEN** they understand that Podman must be installed for ephemeral containers

### Requirement: Blog post Podman limitation

The blog post "Current Limitations" section SHOULD clarify that the "Podman required" limitation applies to ephemeral containers only, not to DevPod persistent workspaces where the provider handles runtime resolution.

Previously: "Podman required: The sandbox uses Podman, not Docker."

#### Scenario: User reads blog post limitations
- **GIVEN** a user is reading the sandbox isolation blog post
- **WHEN** they read the current limitations section
- **THEN** they understand that Podman is required only for ephemeral containers, and DevPod workspaces use a configured provider

## REMOVED Requirements

### Requirement: Standalone Podman binary as universal prerequisite

Documentation MUST NOT state that users need `podman` in `$PATH` as a blanket prerequisite for all sandbox usage. The standalone binary requirement applies only to ephemeral containers, not DevPod workspaces.

Reason: The upstream `LookPath("podman")` pre-flight check was removed from the DevPod code path. The docker provider aliased as `podman` (configured by `uf setup`) handles container runtime resolution without requiring the standalone binary.
