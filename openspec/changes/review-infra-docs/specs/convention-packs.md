## ADDED Requirements

### Requirement: Convention Packs Reference Page

A new reference page MUST be created at `content/docs/reference/convention-packs.md` that documents the convention pack system. The page MUST explain what convention packs are, how they are structured, how they are loaded, and what severity levels mean.

The page MUST include:

- Definition of convention packs as portable, severity-classified coding standards
- Explanation of the three severity levels (MUST, SHOULD, MAY) and their enforcement implications
- Description of how packs are loaded by heroes (directory convention: `.opencode/uf/packs/`)
- List of all available pack types (default, content, go, typescript, python, CI, severity) with brief descriptions, plus their `-custom` variants
- The CI convention pack (#206) documented as an example, including its scope (CI workflow authoring, action pinning, SHA verification) and rule count
- Cross-reference to the constitution governance hierarchy section

The page MUST use standard Hugo frontmatter with `weight: 30` to appear after existing reference pages (cli.md uses weight 10).

#### Scenario: User looks up convention pack severity levels

- **GIVEN** a user encounters a "MUST" violation in a review finding
- **WHEN** the user navigates to the Convention Packs reference page
- **THEN** the page explains that MUST violations are mandatory rules that block merging, SHOULD violations are recommended practices, and MAY violations are optional suggestions

#### Scenario: User discovers available convention packs

- **GIVEN** a contributor wants to know what coding standards apply to their project
- **WHEN** the user reads the Convention Packs reference page
- **THEN** the page lists all available pack types with brief descriptions and explains how packs are loaded from `.opencode/uf/packs/`

### Requirement: Developer Guide Convention Pack Cross-Reference

The developer guide (`content/docs/getting-started/developer.md`) MUST be updated to cross-reference the new Convention Packs reference page. If a convention pack section already exists, it SHOULD be expanded with a link to the reference page. If no section exists, one MUST be added.

#### Scenario: Developer guide links to convention pack reference

- **GIVEN** a developer is reading the developer guide
- **WHEN** they reach the convention pack section
- **THEN** the section provides a brief overview and links to the full Convention Packs reference page for details

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
