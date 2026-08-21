# Delta Spec: Project Page (`content/docs/projects/dewey.md`)

Issues: #41, #113, #186, #131

## ADDED Requirements

### Requirement: RPM Installation Option

The installation section MUST include an RPM installation method for Fedora/RHEL/CentOS systems, positioned after Homebrew and before `go install`.

#### Scenario: User views installation options
- **GIVEN** a user visits the Dewey project page
- **WHEN** they read the installation section
- **THEN** they see RPM listed as an installation method alongside Homebrew and Linux binary

### Requirement: Curated Knowledge Stores Feature

The key features list MUST include curated knowledge stores as a capability, referencing the `dewey curate` command.

#### Scenario: User reviews Dewey capabilities
- **GIVEN** a user reads the key features section
- **WHEN** they scan the feature list
- **THEN** they see curated knowledge stores listed with a description of what it provides

### Requirement: Pluggable Provider Feature

The key features list MUST mention pluggable embedding and synthesis providers (Ollama and Vertex AI).

#### Scenario: User evaluates provider support
- **GIVEN** a user wants to know which AI providers Dewey supports
- **WHEN** they read the key features section
- **THEN** they see that both Ollama and Vertex AI are supported as configurable providers

### Requirement: Content Sanitization Feature

The key features list SHOULD mention content sanitization as a security capability.

#### Scenario: User evaluates security features
- **GIVEN** a user is concerned about untrusted content in their knowledge base
- **WHEN** they read the key features section
- **THEN** they see content sanitization listed with a brief description

## MODIFIED Requirements

### Requirement: Tool Count

The tool count MUST be updated from 48 to 50 in all locations where it appears on the page.

Previously: "48 MCP tools"

#### Scenario: Tool count accuracy
- **GIVEN** a user reads the Dewey project page
- **WHEN** they see a reference to MCP tool count
- **THEN** the count reads 50 (not 48)

## REMOVED Requirements

None.
