## Why

Issue [#205](https://github.com/unbound-force/website/issues/205) tracks the Speckit branch naming convention change from `NNN-<name>` to `speckit/NNN-<name>`. The website currently presents the old format in contributor guidance, workflow documentation, and the pipeline article, which can lead users to create branches that no longer match the current Speckit tooling.

## What Changes

- Update documented Speckit branch examples to use `speckit/NNN-<name>`.
- Explain that the `speckit/` prefix applies to newly created feature branches.
- State that existing branches are not renamed and remain usable.
- Keep OpenSpec's existing `opsx/<name>` convention distinct and unchanged.
- Backport `speckit/` prefix handling into the vendored `.specify/scripts/bash` validation and scaffolding scripts so the documented convention passes the repository's own branch gate.
- Review all matching website documentation references so the published guidance is internally consistent.

## Capabilities

### New Capabilities

- `speckit-branch-convention`: Users can find the current Speckit branch prefix and compatibility guidance, and the repository's Speckit scripts accept that prefix.

### Modified Capabilities

- `contributor-workflow`: Contributor instructions use the `speckit/NNN-<name>` format.
- `speckit-pipeline-documentation`: The pipeline article and developer workflow references describe the current branch naming convention.

### Removed Capabilities

- None. This change removes only obsolete examples, not supported workflow behavior.

## Impact

- Affected content includes `AGENTS.md`, the contributing documentation, the Speckit pipeline article, and any other user-facing pages found during implementation that document the old format.
- The vendored `.specify/scripts/bash` validation and scaffolding scripts are backported with `speckit/` prefix handling so the documented convention resolves to `specs/NNN-<name>/` in this repository. No Hugo templates or CI configuration changes are required.
- The implementation must preserve links, frontmatter, navigation, and the distinction between Speckit and OpenSpec branch namespaces.
- Validation consists of searching for stale branch examples, running the website build, and smoke-testing the branch gate on a `speckit/NNN-<name>` branch.

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: PASS

The change improves the self-describing workflow artifacts that contributors use to coordinate work. It does not introduce runtime coupling between heroes or tools.

### II. Composability First

**Assessment**: PASS

This change updates documentation and backports branch-prefix handling into the vendored Speckit scripts. It adds no mandatory dependency: Speckit and OpenSpec remain independently documented and usable, and the scripts continue to accept legacy branches.

### III. Observable Quality

**Assessment**: PASS

The updated documentation provides an explicit, machine-searchable convention and compatibility statement. The change is validated through repository-wide reference checks and a successful Hugo build.

### IV. Testability

**Assessment**: PASS

The change has isolated validation criteria: stale-reference searches, preservation of required examples, and the existing production build. No external service is required to verify the documentation.
