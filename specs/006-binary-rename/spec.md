# Feature Specification: Binary Rename -- Website Updates

**Feature Branch**: `006-binary-rename`  
**Created**: 2026-03-22  
**Status**: Implemented  
**Input**: User description: "Rename unbound CLI binary to unbound-force / uf (avoids collision with NLnet Labs DNS resolver)"

## User Scenarios & Testing _(mandatory)_

### User Story 1 - Developer Reads Accurate CLI Commands (Priority: P1)

A developer visiting the getting-started guide needs to see the correct CLI binary name so they can install and use the Unbound Force toolchain without confusion. Currently, the site references the `unbound` binary name, which collides with the NLnet Labs Unbound DNS resolver available via `brew install unbound`. After the upstream binary rename (Spec 013 in the meta repo), the website must reflect the new `unbound-force` binary name and its `uf` short alias so developers follow working instructions.

**Why this priority**: If the documented CLI commands are wrong, no developer can successfully set up their environment. This is the single highest-impact change -- incorrect install and setup commands render the entire getting-started experience broken.

**Independent Test**: Can be fully tested by navigating to the Developer Environment Setup page, following every CLI command listed, and confirming each one executes correctly against the renamed binary. Delivers immediate value by unblocking new developers.

**Acceptance Scenarios**:

1. **Given** a developer visits the Developer Environment Setup page, **When** they read the Homebrew install command, **Then** the command reads `brew install unbound-force/tap/unbound-force` (not `brew install unbound-force/tap/unbound`).
2. **Given** a developer follows the setup instructions, **When** they encounter CLI subcommands, **Then** all references use `uf` as the primary command name (e.g., `uf setup`, `uf doctor`, `uf init`).
3. **Given** a developer reads the page for the first time, **When** the `uf` alias is introduced, **Then** the page explains that `uf` is a short alias for `unbound-force` so the reader understands both names.
4. **Given** a developer has the NLnet Labs Unbound DNS resolver installed, **When** they follow the website instructions, **Then** no command conflicts with the DNS resolver binary.

---

### User Story 2 - Developer Follows Accurate Workflow Documentation (Priority: P2)

A developer reading the Common Workflows page needs the environment setup workflow section to reflect the renamed binary. This page documents the step-by-step flow for setting up a new project, including Homebrew installation, `uf setup`, and `uf doctor` commands.

**Why this priority**: The common workflows page is the second most-visited page for environment setup after the developer guide. Incorrect commands here create a confusing experience where one page contradicts another.

**Independent Test**: Can be fully tested by reading the Environment Setup section of the Common Workflows page and confirming every CLI command matches the renamed binary. No other page content is affected.

**Acceptance Scenarios**:

1. **Given** a developer reads the Environment Setup workflow, **When** they see Homebrew install commands, **Then** the command uses `brew install unbound-force/tap/unbound-force`.
2. **Given** a developer reads the step-by-step workflow, **When** CLI subcommands are listed, **Then** all references use `uf` (e.g., `uf setup`, `uf doctor`, `uf init`).

---

### User Story 3 - Product Manager Reads Correct Install Instructions (Priority: P3)

A product manager reading the Product Manager guide needs to see accurate Homebrew install instructions for the CLI package. The current page references `brew install unbound-force/tap/unbound` and describes the package as `unbound`. These must be updated to reflect the renamed binary.

**Why this priority**: Product managers are a secondary audience for CLI installation. The change is small (2 references) but must be consistent with the rest of the site to avoid confusion.

**Independent Test**: Can be fully tested by reading the Product Manager page and confirming the Homebrew install command and package name reference are correct.

**Acceptance Scenarios**:

1. **Given** a product manager reads the installation section, **When** they see the Homebrew install command, **Then** it reads `brew install unbound-force/tap/unbound-force`.
2. **Given** a product manager reads the installation context, **When** the CLI package is mentioned by name, **Then** it is referred to as `unbound-force` (with `uf` alias noted), not `unbound`.

---

### Edge Cases

- What happens if a page references both the organization name "Unbound Force" and the CLI binary `uf` / `unbound-force` in close proximity? The content must clearly distinguish between the organization name and the CLI tool name through formatting (e.g., inline code backticks for the CLI binary, plain text for the org name).
- What happens if a new getting-started page is added in the future that references CLI commands? The contributing guidelines and content conventions should make it clear that the CLI binary is `uf` / `unbound-force`, never `unbound`.

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: All Homebrew install commands across the website MUST use `brew install unbound-force/tap/unbound-force` instead of `brew install unbound-force/tap/unbound`.
- **FR-002**: All CLI subcommand references MUST use `uf` as the primary command name (e.g., `uf setup`, `uf doctor`, `uf init`) instead of `unbound setup`, `unbound doctor`, `unbound init`.
- **FR-003**: The first mention of the `uf` command on each page MUST include a parenthetical or inline note explaining that `uf` is a short alias for `unbound-force`, so readers encountering either name understand they are the same tool.
- **FR-004**: All inline code references to the CLI binary MUST use backtick formatting (e.g., `uf`, `unbound-force`) to visually distinguish the tool name from the "Unbound Force" organization name in prose.
- **FR-005**: References to the organization name "Unbound Force" (not the CLI binary) MUST remain unchanged -- only CLI binary/command references are updated.
- **FR-006**: The website MUST build successfully (`npm run build`) with zero errors after all content updates are applied.
- **FR-007**: No page on the built website MUST contain the bare `unbound` command as a CLI reference (only `unbound-force` or `uf` are acceptable for CLI usage).

### Assumptions

- The upstream binary rename (Spec 013 in the `unbound-force/unbound-force` meta repo) is completed and merged before this website work is deployed, so the documented commands actually work.
- The Homebrew tap formula has been updated to `unbound-force` with a `uf` symlink before the website changes go live.
- The `uf` alias is the preferred short form for documentation. `unbound-force` is the canonical full name used in Homebrew and formal references. Day-to-day documentation prefers `uf` for brevity.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: Zero instances of the bare `unbound` CLI command remain in any published website content (verified by searching the built site output for `unbound` CLI patterns while excluding organization name references).
- **SC-002**: 100% of CLI command examples across all getting-started pages use the `uf` command name and produce correct output when copy-pasted into a terminal with the renamed binary installed.
- **SC-003**: The site builds and deploys without errors after all updates are applied.
- **SC-004**: A new developer following the getting-started guide from scratch can install and set up the toolchain in under 10 minutes without encountering any incorrect or ambiguous CLI command references.
- **SC-005**: No naming confusion exists between the Unbound Force CLI tool (`uf` / `unbound-force`) and the NLnet Labs Unbound DNS resolver in any website content.
