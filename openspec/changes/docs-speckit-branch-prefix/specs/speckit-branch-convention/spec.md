## ADDED Requirements

### Requirement: Current Speckit branch prefix is documented

Contributor and workflow documentation MUST identify newly created Speckit feature branches using the `speckit/NNN-<short-name>` format.

#### Scenario: Contributor reads branch setup guidance

- **GIVEN** a contributor opens the website's contributor or developer workflow guidance
- **WHEN** the guidance explains how to create a Speckit feature branch
- **THEN** it MUST show an example beginning with `speckit/` followed by the three-digit feature number and short name

### Requirement: Existing branch compatibility is documented

The documentation MUST state that the convention applies to new branches and that existing branches are not renamed.

#### Scenario: Contributor has an existing numbered branch

- **GIVEN** a contributor is working on an existing branch named `NNN-<short-name>`
- **WHEN** they read the branch convention guidance
- **THEN** the documentation MUST NOT require them to rename that branch

### Requirement: Speckit and OpenSpec namespaces remain distinct

Documentation MUST continue to identify OpenSpec branches with the `opsx/<change-name>` format and MUST NOT replace that format with the Speckit prefix.

#### Scenario: Contributor compares the two workflows

- **GIVEN** a page documents both Speckit and OpenSpec workflows
- **WHEN** it lists their branch conventions
- **THEN** Speckit MUST use `speckit/NNN-<short-name>` and OpenSpec MUST use `opsx/<change-name>`

### Requirement: Branch examples are internally consistent

All current website and repository contributor guidance that describes Speckit branch names MUST use the new prefix, while legitimate numbered directory paths and clearly labeled historical references MAY retain their original form.

#### Scenario: Repository guidance is reviewed after the update

- **GIVEN** a reviewer searches current contributor and website workflow guidance
- **WHEN** they inspect each Speckit branch example
- **THEN** every current example MUST use `speckit/NNN-<short-name>`, no current example MAY present the unprefixed form as recommended, and existing unprefixed branches MUST remain documented as usable
