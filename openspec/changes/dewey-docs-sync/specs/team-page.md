# Delta Spec: Team Page (`content/docs/team/dewey.md`)

Issues: #41, #113, #208

## ADDED Requirements

### Requirement: Curate Tool in Tool Catalog

The tool catalog MUST include the `curate` tool in the appropriate category (Knowledge Management).

#### Scenario: User reviews available tools
- **GIVEN** a user reads the tool catalog on the team page
- **WHEN** they look at Knowledge Management tools
- **THEN** they see `curate` listed with a brief description

### Requirement: Store Compiled Tool in Tool Catalog

The tool catalog MUST include the `store_compiled` tool in the Knowledge Management category (alongside `compile`, `lint`, and `promote`).

#### Scenario: User reviews compiled article tools
- **GIVEN** a user wants to persist compiled knowledge articles
- **WHEN** they look at the tool catalog
- **THEN** they see `store_compiled` listed with a brief description

### Requirement: Pluggable Providers Mention

The embedding model section SHOULD mention that Dewey supports pluggable providers (Ollama and Vertex AI), not just the default IBM Granite model.

#### Scenario: User evaluates embedding options
- **GIVEN** a user reads the embedding model section
- **WHEN** they want to know about alternative providers
- **THEN** they see a note about pluggable provider support with a link to the getting-started guide

## MODIFIED Requirements

### Requirement: Tool Count

The tool count MUST be updated from 48 to 50 in the tool catalog header and any other location where the count appears.

Previously: "48 tools across 12 categories"

#### Scenario: Tool count consistency
- **GIVEN** a user reads the team page
- **WHEN** they see the tool count
- **THEN** it reads 50 (not 48)

### Requirement: Embedding Chunk Size Configuration

The `max_chunk_chars` documentation MUST also reference the `DEWEY_CHUNK_MAX_CHARS` environment variable as an alternative configuration method. Note: this env var is already documented at line 136 of the current page. Verify the existing content is accurate and consistent with #208 rather than duplicating.

Previously: `DEWEY_CHUNK_MAX_CHARS` env var is already mentioned (line 136). Verify accuracy against #208.

#### Scenario: User configures chunk size via env var
- **GIVEN** a user wants to set chunk size without editing config.yaml
- **WHEN** they read the embedding model section
- **THEN** they see `DEWEY_CHUNK_MAX_CHARS` mentioned alongside the config field

## REMOVED Requirements

None.
