## ADDED Requirements

### Requirement: gateway-docs-page

The website MUST have a gateway documentation page covering all `uf gateway` subcommands and the credential isolation model.

#### Scenario: user visits gateway docs

- **GIVEN** a user navigates to the gateway documentation page
- **WHEN** the page renders
- **THEN** all 3 subcommands MUST be documented: `start`, `stop`, `status`
- **AND** the `start` flags (`--port`, `--provider`, `--detach`) MUST be listed
- **AND** the credential isolation model MUST be explained

### Requirement: gateway-provider-docs

The gateway page MUST document all supported providers and the auto-detection mechanism.

#### Scenario: user configures provider

- **GIVEN** a user reads the provider section
- **WHEN** they check which providers are supported
- **THEN** Vertex AI, Bedrock, and Anthropic MUST be listed
- **AND** the auto-detection priority order (Vertex > Bedrock > Anthropic) MUST be documented
- **AND** the environment variables scanned for each provider MUST be listed

### Requirement: gateway-vertex-translation-docs

The gateway page MUST document Vertex AI-specific translation behavior.

#### Scenario: Vertex AI user reads translation details

- **GIVEN** a Vertex AI user reads the Vertex-specific section
- **WHEN** they review the translation behavior
- **THEN** the following MUST be documented: model field removal, `anthropic_version` injection, `streamRawPredict` vs `rawPredict` selection, SSE event filtering (drops `vertex_event`/`ping`), header stripping (`anthropic-beta`/`anthropic-version`)
- **AND** the synthetic model catalog (9 models with capabilities) SHOULD be mentioned

### Requirement: gateway-token-refresh-docs

The gateway page MUST document the token refresh behavior.

#### Scenario: user reads token lifecycle

- **GIVEN** a user reads the token refresh section
- **WHEN** they review the refresh mechanism
- **THEN** the proactive refresh within 5 minutes of expiry MUST be documented
- **AND** the deduplication mechanism SHOULD be mentioned

### Requirement: gateway-sandbox-integration-docs

The gateway page MUST document sandbox integration with a cross-reference to the sandbox docs page.

#### Scenario: user reads sandbox integration

- **GIVEN** a user reads the gateway-sandbox integration section
- **WHEN** they review the auto-start behavior
- **THEN** it MUST explain that the gateway auto-starts when a cloud provider is detected during `uf sandbox start`
- **AND** it MUST explain that `ANTHROPIC_BASE_URL` and `ANTHROPIC_AUTH_TOKEN` are passed to the container instead of credential mounts

## MODIFIED Requirements

_None._

## REMOVED Requirements

_None._
