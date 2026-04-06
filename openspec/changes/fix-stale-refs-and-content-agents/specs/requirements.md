## ADDED Requirements

### Requirement: Content Agent Documentation

The Divisor team page MUST document 3 content creation personas (The Scribe, The Herald, The Envoy) with their role descriptions and temperature settings.

#### Scenario: Visitor reads content agents

- **GIVEN** a visitor on the Divisor team page
- **WHEN** they look for content creation capabilities
- **THEN** they find descriptions of The Scribe (tech docs, 0.1), The Herald (blog/announcements, 0.4), and The Envoy (PR/comms, 0.5)

### Requirement: Convention Pack Completeness

The developer guide convention packs table MUST list 9 files including `content.md` (tool-owned, writing standards) and `content-custom.md` (user-owned stub).

#### Scenario: Developer checks pack files

- **GIVEN** a developer reading the convention packs section
- **WHEN** they count the pack files in the table
- **THEN** they find 9 files listed

### Requirement: Replicator in Contributing

The contributing page MUST list the Replicator repository for issue reporting.

#### Scenario: Contributor looks for Replicator issues

- **GIVEN** a contributor on the contributing page
- **WHEN** they look for where to report Replicator issues
- **THEN** they find a link to `github.com/unbound-force/replicator/issues`

## MODIFIED Requirements

### Requirement: Bun Removal from Setup Lists

The development tools list on quick-start.md, developer.md, and common-workflows.md MUST NOT include "Bun." Previously: "Node.js, Bun, OpenSpec CLI, Replicator."

#### Scenario: Developer reads setup tools

- **GIVEN** a developer reading any setup documentation page
- **WHEN** they look at the development tools list
- **THEN** "Bun" does not appear

### Requirement: Hivemind to Dewey

All references to "Hivemind" in user-facing content MUST be replaced with "Dewey." Previously: 6 references to "Hivemind" across common-workflows.md, unleash-in-practice.md, and the-divisor.md.

#### Scenario: Reader searches for Hivemind

- **GIVEN** a reader searching the website content (excluding historical specs)
- **WHEN** they search for "Hivemind"
- **THEN** zero results are found

### Requirement: Divisor Persona Count

The Divisor team page and team index MUST reflect 8 canonical personas (5 review + 3 content). Previously: "five canonical roles."

#### Scenario: Visitor reads Divisor description

- **GIVEN** a visitor on the Divisor team page
- **WHEN** they read the council structure description
- **THEN** the count reflects 8 personas

### Requirement: Session Ritual Update

The Session Ritual section in developer.md MUST reference current workflow patterns (`/finale`) instead of stale MCP tool calls (`hive_ready()`, `hive_sync()`).

#### Scenario: Developer reads session lifecycle

- **GIVEN** a developer reading the Session Ritual section
- **WHEN** they look for end-of-session steps
- **THEN** they find `/finale` referenced, not `hive_ready()`/`hive_sync()`

### Requirement: .hive/ Initialization Attribution

The common-workflows.md setup section MUST NOT list ".hive/ initialization" as a separate `uf setup` step. Previously: listed alongside Development tools.

#### Scenario: Developer reads setup steps

- **GIVEN** a developer reading the environment setup section
- **WHEN** they look at what `uf setup` installs
- **THEN** ".hive/ initialization" is not listed as a separate tool (it's handled by `replicator init` via `uf init`)

## REMOVED Requirements

_(None — all changes are additions or modifications)_
