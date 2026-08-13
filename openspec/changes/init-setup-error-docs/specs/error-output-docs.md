## ADDED Requirements

### Requirement: init-error-output-documentation

The CLI reference page MUST document that `uf init` displays the actual error output from any failing sub-tool, rather than a generic summary message.

#### Scenario: user reads init command documentation

- **GIVEN** a user visits the CLI Reference page
- **WHEN** they read the `init` command section
- **THEN** the section MUST include a note that sub-tool failures display the actual error output from the failing command

### Requirement: setup-error-output-documentation

The CLI reference page MUST document that `uf setup` displays the actual error output from any failing sub-tool, rather than a generic summary message.

#### Scenario: user reads setup command documentation

- **GIVEN** a user visits the CLI Reference page
- **WHEN** they read the `setup` command section
- **THEN** the section MUST include a note that sub-tool failures display the actual error output from the failing command

### Requirement: developer-guide-error-output-note

The developer guide MUST note in the Sub-Tool Initialization section that `uf init` surfaces actual error output when a sub-tool step fails.

#### Scenario: user reads sub-tool initialization section

- **GIVEN** a user visits the Developer Guide page
- **WHEN** they read the Sub-Tool Initialization section
- **THEN** the section MUST include a note that if any initialization step fails, the actual error output from the failing tool is displayed

## MODIFIED Requirements

_None._

## REMOVED Requirements

_None._
