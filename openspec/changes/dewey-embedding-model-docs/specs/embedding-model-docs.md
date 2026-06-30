## ADDED Requirements

### Requirement: DEWEY_CHUNK_MAX_CHARS Environment Variable Documentation

The getting started guide (`content/docs/getting-started/knowledge.md`) MUST document the `DEWEY_CHUNK_MAX_CHARS` environment variable in the Embedding Model Alignment section. The documentation MUST include the variable name, default value (12288), and a description stating it overrides the configured max chunk characters for embedding.

#### Scenario: User looks up DEWEY_CHUNK_MAX_CHARS in docs
- **GIVEN** a user has encountered the `DEWEY_CHUNK_MAX_CHARS` env var in Dewey's source or help output
- **WHEN** they navigate to the Knowledge Retrieval getting started guide
- **THEN** they find `DEWEY_CHUNK_MAX_CHARS` documented in the Embedding Model Alignment section with its default value and purpose

### Requirement: max_chunk_chars Config Field Documentation

The team page (`content/docs/team/dewey.md`) MUST add `max_chunk_chars` to the existing `embedding:` YAML config example in the Embedding Model section. The field SHOULD include a comment explaining its purpose and default value.

#### Scenario: User configures embedding chunk size via config
- **GIVEN** a user wants to customize the embedding chunk size
- **WHEN** they read the Embedding Model section on the Dewey team page
- **THEN** they find `max_chunk_chars` in the YAML example with a comment indicating the default (12288) and its effect

### Requirement: Legacy Model Advisory Documentation

The getting started guide (`content/docs/getting-started/knowledge.md`) MUST document the `dewey doctor` legacy model advisory in the Diagnostic and Maintenance Commands section. The documentation MUST explain that an informational note appears when `granite-embedding:30m` is configured, suggesting an upgrade to the Granite Embedding R2 model when it becomes available.

#### Scenario: User sees legacy model advisory in dewey doctor output
- **GIVEN** a user runs `dewey doctor` and sees an informational advisory about the legacy embedding model
- **WHEN** they check the website documentation for explanation
- **THEN** they find a description of the advisory in the `dewey doctor` section explaining it is informational and that R2 is a future upgrade

### Requirement: R2 Upgrade Path Mention

The project page (`content/docs/projects/dewey.md`) and team page (`content/docs/team/dewey.md`) SHOULD mention the Granite Embedding R2 model as a future upgrade path in their respective Embedding Model or Semantic Search sections. The language MUST NOT present R2 as currently available or as an action item -- it SHOULD use forward-looking language indicating a future default change.

#### Scenario: User reads about Dewey's embedding model
- **GIVEN** a user is reading about Dewey's embedding model on the project or team page
- **WHEN** they review the embedding model details
- **THEN** they see a note indicating that a future release will update the default to Granite Embedding R2

## MODIFIED Requirements

### Requirement: Embedding Config YAML Example

The existing `embedding:` YAML config example in `content/docs/team/dewey.md` (lines 129-133) MUST be extended to include the `max_chunk_chars` field. Previously, the example only showed `provider`, `model`, and a comment about alternatives.

#### Scenario: Config example includes chunk size
- **GIVEN** a user reads the Embedding Model section on the team page
- **WHEN** they view the YAML config example
- **THEN** the example includes `max_chunk_chars: 12288` with an explanatory comment, in addition to the existing `provider` and `model` fields

### Requirement: dewey doctor Diagnostic Table

The existing `dewey doctor` diagnostic table in `content/docs/getting-started/knowledge.md` (lines 380-388) MUST be updated to reflect that the Embedding Layer section now also checks for legacy model advisories. Previously, the Embedding Layer row only mentioned Ollama availability and model status.

#### Scenario: Doctor table reflects legacy model check
- **GIVEN** a user reviews the `dewey doctor` diagnostic table in the getting started guide
- **WHEN** they look at the Embedding Layer row
- **THEN** the description mentions legacy model advisory detection alongside Ollama availability and model status

## REMOVED Requirements

None. All changes are additive or modifications to existing content.
