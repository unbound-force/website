## ADDED Requirements

### Requirement: Constitution Principle V Documentation

The constitution page (`content/docs/getting-started/constitution.md`) MUST be updated to include Principle V: Security by Default. The new section MUST follow the same structure as Principles I-IV:

- Principle heading and description
- MUST rules
- SHOULD rules (if applicable)
- Rationale

The section MUST cover the upstream constitution's actual Principle V content:

- Supply chain integrity (dependencies MUST be verified by content hash)
- Input validation (all external inputs MUST be validated and sanitized)
- Least privilege (components MUST operate with minimum permissions)
- Dependency justification (external dependencies MUST be justified)
- The principle's relationship to the existing four principles

The upstream constitution (`.specify/memory/constitution.md`) is the authoritative source for Principle V content. The spec's description above is indicative — if the upstream constitution defines Principle V differently, the upstream version takes precedence.

The page frontmatter description MUST be updated to reference 5 principles instead of 4. The constitution version reference MUST be reconciled with the upstream version (website currently shows v1.1.0; verify upstream version and update accordingly).

#### Scenario: User reads the constitution and finds all five principles

- **GIVEN** a contributor is reviewing the Unbound Force constitution
- **WHEN** they read the constitution page
- **THEN** they find Principles I through V, with Principle V (Security by Default) following the same structure as the others

#### Scenario: User understands Security by Default requirements

- **GIVEN** a developer is integrating external dependencies into their project
- **WHEN** they read the Principle V section on the constitution page
- **THEN** they understand that dependencies must be hash-verified, inputs validated, permissions minimized, and dependencies justified

## MODIFIED Requirements

### Requirement: Branch Naming Convention Update

All website content pages containing the old `NNN-<name>` branch naming pattern MUST be updated to use `speckit/NNN-<name>`. This includes at minimum:

- `content/docs/getting-started/common-workflows.md`
- `content/docs/getting-started/developer.md` (line 130: "Speckit uses `NNN-<short-name>` branches")

Previously: Branch names follow the pattern `NNN-<name>` (e.g., `013-binary-rename`).

Updated: Branch names follow the pattern `speckit/NNN-<name>` (e.g., `speckit/013-binary-rename`).

All instances of the old pattern in each page MUST be updated. No new content is added — this is a corrective find-and-replace. Note: AGENTS.md also contains the old pattern but is out of scope for this change (AGENTS.md is a project governance file, not website content).

#### Scenario: User reads about branch naming

- **GIVEN** a contributor is learning about the Speckit workflow branch conventions
- **WHEN** they read the common workflows page or developer guide
- **THEN** all branch name examples use the `speckit/NNN-<name>` prefix format

### Requirement: Constitution Page Metadata Update

The constitution page frontmatter `description` field MUST be updated from referencing "4 core principles" to "5 core principles" to reflect the addition of Principle V.

Previously: `description: "The 4 core principles that govern all Unbound Force heroes..."`

Updated: `description: "The 5 core principles that govern all Unbound Force heroes..."`

#### Scenario: Search results show correct principle count

- **GIVEN** a user searches for "Unbound Force constitution"
- **WHEN** the constitution page appears in results
- **THEN** the description mentions 5 core principles, not 4

## REMOVED Requirements

None.
