## ADDED Requirements

### Requirement: config-docs-page

The website MUST have a configuration documentation page covering all `uf config` subcommands, layered loading, and configuration sections.

#### Scenario: user visits config docs

- **GIVEN** a user navigates to the configuration documentation page
- **WHEN** the page renders
- **THEN** all 3 subcommands MUST be documented: `init`, `show`, `validate`
- **AND** each subcommand MUST include usage example and description

### Requirement: config-layered-loading-docs

The configuration page MUST explain the layered loading system with precedence rules.

#### Scenario: user checks config precedence

- **GIVEN** a user reads the layered loading section
- **WHEN** they review the precedence rules
- **THEN** the 3 layers MUST be listed: user config (`~/.config/uf/config.yaml`), repo config (`.uf/config.yaml`), environment variables
- **AND** the precedence order MUST be clearly stated (user > repo > env)

### Requirement: config-sections-docs

The configuration page MUST document the 7 configuration sections.

#### Scenario: user looks up a config section

- **GIVEN** a user reads the configuration sections
- **WHEN** they scan the section overview
- **THEN** all 7 sections MUST be listed: setup, scaffold, embedding, sandbox, gateway, doctor, workflow
- **AND** each section MUST include a brief description and key settings

### Requirement: config-common-customizations

The configuration page SHOULD include common customization examples.

#### Scenario: Fedora user sets package manager

- **GIVEN** a Fedora/RHEL user reads the common customizations section
- **WHEN** they look for platform-specific setup
- **THEN** a YAML example MUST show `setup.package_manager: dnf`

## MODIFIED Requirements

### Requirement: common-workflows-config-reference

The `common-workflows.md` page MUST reference `uf config` commands alongside existing `.uf/config.yaml` mentions.

Previously: The page mentioned `.uf/config.yaml` without referencing the commands that manage it.

#### Scenario: user reads workflow config section

- **GIVEN** a user reads the workflow configuration section in common-workflows
- **WHEN** they see `.uf/config.yaml` mentioned
- **THEN** a reference to `uf config init` and `uf config validate` MUST be present
- **AND** a link to the full configuration docs page SHOULD be included

## REMOVED Requirements

_None._
