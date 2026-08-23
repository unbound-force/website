## ADDED Requirements

### Requirement: Development Environment Setup Guide

The website MUST include a getting-started guide at `content/docs/getting-started/devenv-setup.md` that walks users through setting up a DevPod-based development environment from prerequisites to first AI agent session.

The page MUST cover:
- Prerequisites: Podman and DevPod installation via `uf setup`
- DevPod initialization: `uf sandbox init` to scaffold `.devcontainer/devcontainer.json`
- Workspace lifecycle: `sandbox create`, `sandbox start`, `sandbox stop`, `sandbox destroy`
- Mount modes: isolated (read-only, default) vs direct (read-write) with guidance on when to use each
- IDE integration: `--ide` flag with supported values (none, vscode, openvscode, fleet, jupyternotebook, cursor) and resolution chain (CLI flag > env var > config > default)
- Gateway auto-start for cloud LLM providers (Vertex AI, Bedrock, Anthropic)
- Platform-specific notes for macOS (Podman machine, virtiofs UID mapping) and Linux (rootless containers, SELinux)
- Troubleshooting: common issues (files appearing as root:nobody, `uf doctor` diagnostics)

The page MUST NOT duplicate full command reference syntax from sandbox.md or cli.md; it SHOULD cross-reference those pages for detailed flag documentation.

#### Scenario: New user sets up DevPod sandbox on macOS
- **GIVEN** a user has installed the `uf` CLI on macOS
- **WHEN** they follow the Development Environment Setup guide
- **THEN** they can run `uf setup` to install Podman and DevPod, run `uf sandbox init` to generate a devcontainer config, and start a sandbox session with `uf sandbox start`

#### Scenario: User selects an IDE for their sandbox session
- **GIVEN** a user has a DevPod sandbox configured
- **WHEN** they start a sandbox with `uf sandbox start --ide vscode`
- **THEN** the guide explains the IDE selection resolution chain and available options

#### Scenario: User on Linux encounters SELinux mount issues
- **GIVEN** a user is running on a Linux system with SELinux enabled
- **WHEN** they encounter permission errors in their sandbox
- **THEN** the guide directs them to the platform-specific notes explaining automatic `:Z` label detection

### Requirement: Multi-Platform Support Guide

The website MUST include a getting-started guide at `content/docs/getting-started/multi-platform.md` that documents `uf init --platform` for scaffolding projects for different AI coding platforms.

The page MUST cover:
- The `--platform` flag with supported values (opencode, cursor)
- Repeatable flag syntax for dual-platform scaffolding (`--platform opencode --platform cursor`)
- OpenCode-specific output: `.opencode/` directory, MCP config, convention packs
- Cursor-specific output: `.cursor/` directory, `.mdc` rule files translated from convention packs, `.cursor/mcp.json`
- Divisor subset mode: `--divisor --platform cursor`
- When to use each platform and how to switch

The page MUST NOT fabricate platform features that are not implemented in the CLI.

#### Scenario: User scaffolds a project for Cursor
- **GIVEN** a user wants to use Cursor as their AI coding platform
- **WHEN** they run `uf init --platform cursor`
- **THEN** the guide explains the Cursor-native output files generated (`.cursor/` directories, `.mdc` rule files, `.cursor/mcp.json`)

#### Scenario: User wants both OpenCode and Cursor support
- **GIVEN** a user works across both OpenCode and Cursor
- **WHEN** they run `uf init --platform opencode --platform cursor`
- **THEN** the guide explains that both platform configurations are generated and can coexist

## MODIFIED Requirements

### Requirement: Sandbox Reference Page (sandbox.md)

Previously: sandbox.md documents Eclipse Che/Dev Spaces as a CDE backend option alongside Podman, and does not fully cover IDE selection or the simplified provider pre-flight.

The sandbox.md reference page MUST be updated to:
- Present DevPod as the primary CDE backend with Che/Dev Spaces marked as legacy/deprecated
- Document the `--ide` flag resolution chain: CLI flag > `UF_SANDBOX_IDE` env var > `.uf/config.yaml` `sandbox.ide` > default `none`
- Document `--provider podman` flag behavior (references registered DevPod provider name, not PATH lookup)
- Reference the diagnostic hint on `devpod up` failure pointing to `uf doctor`/`uf setup`
- Add cross-reference to the `uf ollama-proxy` command for GPU-less workflows within sandbox sessions

#### Scenario: User encounters deprecated Che backend reference
- **GIVEN** a user reads the sandbox reference page
- **WHEN** they see the Eclipse Che/Dev Spaces section
- **THEN** it is clearly marked as legacy/deprecated with a note about the migration error when using `--backend che`

### Requirement: CLI Reference Page (cli.md)

Previously: cli.md does not include the `uf ollama-proxy` command and has an incomplete sandbox flag set.

The cli.md reference page MUST be updated to:
- Add the `uf ollama-proxy` command with subcommands (start, stop, status)
- Document ollama-proxy configuration: port, embed model, gateway URL
- Update `uf sandbox` subcommands to include `--ide` and `--provider` flags
- Update `uf setup` to note Podman and DevPod installation steps (14-16)
- Update `uf doctor` to note Podman and DevPod health checks

#### Scenario: User looks up ollama-proxy command syntax
- **GIVEN** a user needs to run the ollama-proxy for GPU-less development
- **WHEN** they visit the CLI reference page
- **THEN** they find `uf ollama-proxy` with start/stop/status subcommands and configuration options

### Requirement: Changelog Update

Previously: Changelog lists v0.12.0 entries but does not cover DevPod provider changes, IDE selection, ollama-proxy, or multi-platform scaffold support.

The changelog MUST be updated with entries for:
- DevPod as primary CDE backend (Che deprecated)
- `--ide` flag for sandbox IDE selection
- `uf ollama-proxy` command
- `uf init --platform` for multi-platform scaffolding
- DevPod provider pre-flight simplification
- Podman/DevPod added to `uf setup` and `uf doctor`

#### Scenario: User checks what changed in recent releases
- **GIVEN** a user visits the changelog page
- **WHEN** they look at the latest release entries
- **THEN** they find entries for DevPod, IDE selection, ollama-proxy, and multi-platform support

## REMOVED Requirements

No requirements are removed. Eclipse Che/Dev Spaces documentation is retained with a legacy/deprecated label rather than removed, since the `--backend che` flag still exists in the CLI.
