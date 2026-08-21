# Delta Spec: Knowledge Page (`content/docs/getting-started/knowledge.md`)

Issues: #207, #208, #209, #213, #240, #243, #249, #186, #132, #131, #41, #113

## ADDED Requirements

### Requirement: RPM Installation Section

The installation section MUST include an RPM subsection for Fedora/RHEL/CentOS, positioned between Homebrew and `go install`. The section MUST document downloading from GitHub Releases and installing with `sudo dnf install ./dewey_<version>_linux_amd64.rpm`. Both `amd64` and `arm64` architectures SHOULD be mentioned.

#### Scenario: RPM installation instructions
- **GIVEN** a user on Fedora/RHEL/CentOS visits the installation section
- **WHEN** they look for their platform's install method
- **THEN** they find RPM instructions with the `dnf install` command and a note about available architectures

### Requirement: DEWEY_CHUNK_MAX_CHARS Environment Variable (Ref: #208)

The environment variables reference MUST document `DEWEY_CHUNK_MAX_CHARS` (default: 12288) with its description (maximum characters per embedding chunk), corresponding config field (`embedding.max_chunk_chars`), and default value. Note: this variable is already partially documented in the Embedding Model Alignment table (lines 48, 55). Verify the existing documentation is accurate and consistent with #208 rather than duplicating content.

#### Scenario: User configures chunk size
- **GIVEN** a user wants to tune embedding chunk size
- **WHEN** they consult the environment variables table
- **THEN** they find `DEWEY_CHUNK_MAX_CHARS` with its description and default value (12288)

### Requirement: DEWEY_SYNTHESIS_ENDPOINT Environment Variable

The environment variables reference MUST document `DEWEY_SYNTHESIS_ENDPOINT` with its description and fallback chain (`DEWEY_SYNTHESIS_ENDPOINT` → `OLLAMA_HOST` → `http://localhost:11434`). A callout MUST note the inverted precedence: for synthesis, `config.yaml` takes highest priority over env vars (opposite of embedding).

#### Scenario: User configures synthesis endpoint
- **GIVEN** a user wants to set a custom synthesis endpoint
- **WHEN** they read the env var documentation
- **THEN** they see the fallback chain and an explicit warning about the inverted precedence relative to embedding

### Requirement: DEWEY_AUTHOR Environment Variable

The environment variables reference MUST document `DEWEY_AUTHOR` with its description (author tag for learning identities in CI/shared environments).

#### Scenario: User sets author for CI
- **GIVEN** a user runs Dewey in CI
- **WHEN** they need learnings attributed to a specific author
- **THEN** they find `DEWEY_AUTHOR` documented with its purpose

### Requirement: Vertex AI Provider Configuration

A provider configuration section MUST be added documenting how to configure Vertex AI alongside Ollama. This MUST include:
- `embedding.provider` and `synthesis.provider` config fields
- Vertex AI setup requirements (`gcloud` auth, project ID, region)
- `region: global` endpoint behavior (uses `aiplatform.googleapis.com` without region prefix)
- Separate config.yaml examples for Ollama and Vertex AI providers

#### Scenario: User configures Vertex AI
- **GIVEN** a user wants to use Vertex AI instead of Ollama
- **WHEN** they read the provider configuration section
- **THEN** they find a complete Vertex AI config.yaml example with project, region, and auth requirements

#### Scenario: User sets region to global
- **GIVEN** a user configures `region: global` for Vertex AI
- **WHEN** they read the provider docs
- **THEN** they understand that `global` routes to the nearest region via `aiplatform.googleapis.com`

### Requirement: Content Sanitization Configuration

A content sanitization subsection MUST be added documenting:
- The 4-layer sanitization pipeline (injection patterns, hash drift, Markdown validation, size anomaly)
- Per-source configuration via `sanitize_mode` (warn/strict/off) and `trust_tier` in `sources.yaml`
- The severity classifications (critical/high/medium)
- That findings are surfaced by `dewey doctor` and `dewey lint`

#### Scenario: User enables strict sanitization
- **GIVEN** a user indexes untrusted external content
- **WHEN** they read the sanitization section
- **THEN** they know how to set `sanitize_mode: strict` and `trust_tier` per-source in `sources.yaml`

### Requirement: Curated Knowledge Stores

A curated knowledge stores subsection MUST be added documenting:
- The `dewey curate` command and its purpose
- The `knowledge-stores.yaml` configuration file
- The `curated` trust tier
- Background curation during `dewey serve`

#### Scenario: User sets up curation
- **GIVEN** a user wants to curate their knowledge base
- **WHEN** they read the curated knowledge stores section
- **THEN** they understand how to configure `knowledge-stores.yaml` and run `dewey curate`

### Requirement: Global Config Path (Ref: #113)

The configuration section MUST document the global config path (`~/.config/dewey/config.yaml`) and how per-vault configs override global settings.

#### Scenario: User sets global defaults
- **GIVEN** a user wants to set default provider config across all vaults
- **WHEN** they read the configuration section
- **THEN** they learn about `~/.config/dewey/config.yaml` and per-vault overrides

### Requirement: Index Pipeline Performance Note (Ref: #207)

The diagnostic commands section or the `dewey index` reference SHOULD include a note about batch embedding and concurrent source fetching for performance context.

#### Scenario: User learns about indexing performance
- **GIVEN** a user runs `dewey index` on a large vault
- **WHEN** they read about the index command
- **THEN** they understand that embedding is batched and sources are fetched concurrently

### Requirement: Vertex AI Curation Defaults (Ref: #209)

The `dewey curate` documentation SHOULD note Vertex AI-specific defaults (16000 max output tokens, 300s timeout) to set user expectations for curation performance.

#### Scenario: User curates with Vertex AI
- **GIVEN** a user runs `dewey curate` with Vertex AI
- **WHEN** curation takes several minutes
- **THEN** documentation explains that large prompt processing may take up to 300s

## MODIFIED Requirements

### Requirement: Doctor Output Reference

The `dewey doctor` command description MUST include the Synthesis Layer section between the Embedding Layer and MCP Server sections, updating the diagnostic section count from 7 to 8. The synthesis layer reports: provider type (ollama/vertex/unconfigured), resolved endpoint, model name, connectivity status, and (for Ollama) model availability.

Previously: `dewey doctor` documentation showed only Embedding Layer and MCP Server sections.

#### Scenario: User runs dewey doctor
- **GIVEN** a user runs `dewey doctor` to diagnose configuration
- **WHEN** they compare their output to the documentation
- **THEN** the docs show the Synthesis Layer section reporting provider type, resolved endpoint, model name, connectivity status, and (for Ollama) model availability

### Requirement: Learning Identity Format

Learning identity examples MUST use the v3.2.0 timestamped format (`{tag}-{YYYYMMDDTHHMMSS}-{author}`) instead of the old sequential format (`{tag}-{sequence}`). A migration note SHOULD explain the change and backward compatibility.

Previously: Examples showed `authentication-3` format.

#### Scenario: User stores a learning
- **GIVEN** a user reads the learning documentation
- **WHEN** they see identity format examples
- **THEN** examples use the timestamped format (e.g., `authentication-20260502T143022-alice`)

### Requirement: Homebrew Install Note

The Homebrew installation section SHOULD note that macOS cask install issues present in v3.1.0 and v3.2.0 have been fixed.

Previously: No mention of cask install reliability.

#### Scenario: User installs via Homebrew on macOS
- **GIVEN** a macOS user previously encountered SHA-256 mismatch errors
- **WHEN** they read the installation docs
- **THEN** they see a note confirming the cask install is fixed

## REMOVED Requirements

None.
