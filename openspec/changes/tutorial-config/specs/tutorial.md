## ADDED Requirements

### Requirement: config-tutorial-page

The website MUST have a tutorial page at `content/docs/getting-started/config-tutorial.md` walking through common environment customization scenarios with `uf config`.

#### Scenario: user follows config tutorial

- **GIVEN** a user navigates to the config tutorial
- **WHEN** the page renders
- **THEN** the tutorial MUST show how to create a config file with `uf config init`
- **AND** the tutorial MUST include at least 3 customization scenarios with YAML code blocks
- **AND** each scenario MUST include a verification step (`uf config show` or `uf config validate`)

### Requirement: config-tutorial-precedence

The tutorial MUST demonstrate layered loading precedence with a concrete example.

#### Scenario: user understands config precedence

- **GIVEN** a user reads the precedence section
- **WHEN** they see two config layers with different values
- **THEN** the tutorial MUST show which value wins and how to verify with `uf config show`

### Requirement: config-tutorial-platform

The tutorial MUST include at least one platform-specific customization example.

#### Scenario: Fedora/RHEL user configures package manager

- **GIVEN** a Fedora/RHEL user reads the tutorial
- **WHEN** they look for platform-specific setup
- **THEN** the tutorial MUST show `setup.package_manager: dnf` as a YAML example

### Requirement: config-tutorial-frontmatter

The tutorial MUST use valid Hugo/Doks docs frontmatter consistent with existing pages.

#### Scenario: tutorial builds correctly

- **GIVEN** the tutorial file exists with frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build MUST succeed with no errors
- **AND** the page MUST appear in the Getting Started sidebar

## MODIFIED Requirements

_None._

## REMOVED Requirements

_None._
