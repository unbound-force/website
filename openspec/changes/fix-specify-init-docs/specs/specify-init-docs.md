## ADDED Requirements

### Requirement: Specify initialization documented in Sub-Tool Initialization

The Sub-Tool Initialization section of `developer.md` MUST document that `uf init` performs specify initialization as a sub-tool step. The documentation MUST describe:

1. That `.specify/` directory is created in the current project directory
2. That initialization operates offline (no network calls)
3. That the configuration targets the OpenCode integration
4. That the step is skipped if `.specify/` already exists

The documentation MUST NOT present `specify init` as a standalone user-facing command.

#### Scenario: Developer reads Sub-Tool Initialization section

- **GIVEN** a developer is reading the "Sub-Tool Initialization" section of `developer.md`
- **WHEN** they review the list of sub-tool initialization steps
- **THEN** they see specify initialization listed between Dewey initialization and opencode.json configuration, describing that `.specify/` is created with Speckit configuration in offline mode

#### Scenario: Developer encounters .specify/ directory after uf init

- **GIVEN** a developer ran `uf init` and sees a `.specify/` directory they didn't expect
- **WHEN** they check the developer guide to understand what created it
- **THEN** the Sub-Tool Initialization section explains that `uf init` creates `.specify/` for the specification framework

### Requirement: CLI reference mentions specify initialization

The `init` section of `cli.md` MUST mention that the command also initializes the Speckit specification framework (creates `.specify/` directory). The description paragraph SHOULD be updated to include this capability.

No new flags SHALL be added to the flag table — `--here`, `--integration`, and `--offline` are internal arguments, not user-facing flags.

#### Scenario: Developer looks up uf init in CLI reference

- **GIVEN** a developer is reading the `init` section in the CLI reference
- **WHEN** they read the command description
- **THEN** the description mentions specify/Speckit initialization alongside the existing scaffolding description

### Requirement: Setup flow summary includes specify initialization

The sub-tool initialization summary in `common-workflows.md` (line 436) MUST include specify initialization alongside the existing Dewey and opencode.json mentions.

#### Scenario: Developer reads the setup flow

- **GIVEN** a developer is following the Environment Setup workflow
- **WHEN** they read the `uf init` summary at the end of setup
- **THEN** the summary mentions specify initialization alongside Dewey init, config.yaml creation, and opencode.json configuration

## MODIFIED Requirements

### Requirement: cli.md init description scope

Previously: "Scaffold the Unbound Force specification framework into the current directory. Creates Speckit templates, scripts, OpenCode commands and agents, Divisor review personas, convention packs, and OpenSpec schema files."

The description SHOULD be updated to also mention that `uf init` initializes the specify sub-tool (creating `.specify/` with Speckit configuration). The existing description covers file scaffolding but omits the sub-tool initialization phase.

## REMOVED Requirements

None.
