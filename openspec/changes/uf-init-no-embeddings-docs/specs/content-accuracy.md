## ADDED Requirements

### Requirement: Post-Init Embedding Workflow Guidance

Every page that documents the `uf init` Dewey initialization sequence MUST include a note that embedding generation is deferred by default, and SHOULD provide guidance on how to trigger embedding generation separately via `dewey index` (without `--no-embeddings`).

#### Scenario: User runs `uf init` and expects semantic search

- **GIVEN** a user has run `uf init` to scaffold a new project
- **WHEN** they read the documentation about what `uf init` creates
- **THEN** the documentation MUST explain that `dewey index` runs with `--no-embeddings` during init, and that semantic search requires a separate `dewey index` run to generate embeddings

#### Scenario: User reads the `--force` flag documentation

- **GIVEN** a user wants to re-initialize their project with `uf init --force`
- **WHEN** they read the CLI reference for the `--force` flag
- **THEN** the documentation MUST note that re-indexing now uses `--no-embeddings` by default, deferring embedding generation

## MODIFIED Requirements

### Requirement: `uf init` Three-Step Sequence Description

Previously: Step 3 of `uf init` Dewey initialization is described as "`dewey index` — indexes all configured sources" without qualification.

Updated: Step 3 MUST be described as "`dewey index --no-embeddings` — indexes all configured sources without generating embeddings (for faster initialization)".

#### Scenario: User reads the `uf init` creates section in knowledge.md

- **GIVEN** a user is reading the "What `uf init` Creates" section
- **WHEN** they reach step 3 of the Dewey initialization sequence
- **THEN** the step MUST read `dewey index --no-embeddings` and MUST explain that this indexes sources without generating embeddings

### Requirement: `--force` Flag Behavior Description

Previously: The `--force` flag in `developer.md` states it will "re-index Dewey workspace" and in `cli.md` states "Overwrite all existing files" without mentioning embedding behavior.

Updated: All `--force` descriptions MUST note that re-indexing uses `--no-embeddings`, deferring embedding generation.

#### Scenario: User reads developer.md Sub-Tool Initialization

- **GIVEN** a user is reading the Sub-Tool Initialization section
- **WHEN** they read about the `--force` flag behavior with Dewey
- **THEN** the description MUST note that `--force` re-indexes with `--no-embeddings`

### Requirement: Setup Workflow Dewey Index Description

Previously: `common-workflows.md` states `uf init` "runs `dewey init` + `dewey index` when Dewey is available" without qualification.

Updated: The setup workflow description MUST note that `dewey index` runs with `--no-embeddings` during init.

#### Scenario: User reads the setup workflow

- **GIVEN** a user is following the setup workflow in common-workflows.md
- **WHEN** they read about what `uf init` does with Dewey
- **THEN** the description MUST mention `--no-embeddings` and explain how to generate embeddings afterward

## REMOVED Requirements

None.
