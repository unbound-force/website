# Spec: Gaze Documentation Update for Multi-Language Support

## ADDED Requirements

### External Analyzer Protocol Section

The Gaze project page MUST include a new section documenting the external analyzer protocol.

**Scenario**: A user working in Python visits the Gaze project page to evaluate whether Gaze can help with their project.

- GIVEN a user visiting the Gaze project page
- WHEN they read the External Analyzers section
- THEN the section contains a description of the JSON-RPC 2.0 protocol, the three-tier discovery mechanism (CLI flag, `.gaze.yaml`, PATH convention), and a link to `docs/protocol.md` for the full protocol specification

### Migration Notes Section

The Gaze project page MUST include a Migration Notes section documenting breaking changes.

- GIVEN a user upgrading from a previous version of Gaze
- WHEN they read the Migration Notes section
- THEN the section documents the old field name `go_version`, the new field name `language_version`, the new `language` field, a code-level description of the change, and the 7 language-neutral `SideEffectType` aliases (`AsyncTaskSpawn`, `FFICall`, `ErrorSignal`, `GeneratorYield`, `ContainerMutation`, `StreamOutput`, `ResourceManagement`)

- GIVEN a user upgrading from a previous version of Gaze
- WHEN they read the Coverage Behavior Change subsection
- THEN the section documents that coverage now reflects the full test suite (not `-short`), the `--test-short` opt-in flag, the `GAZE_COVERAGE_RUN=1` environment variable, and that CRAP scores may change after upgrade

### Expanded Side Effect Taxonomy

The Gaze project page MUST document the expanded side effect taxonomy with universal types.

- GIVEN a user reviewing Gaze's side effect detection capabilities
- WHEN they read the Universal Side Effect Types subsection
- THEN the section lists the new universal types by tier: `ErrorSignal` (P0), `GeneratorYield`/`ContainerMutation`/`StreamOutput` (P1), and P2 universal types, with a link to protocol documentation for the complete reference

The specific type names MUST be sourced from the merged upstream PR (gaze#184); the names listed here are representative of the expected taxonomy at time of writing.

### Tester Guide `--test-short` Documentation

The tester guide MUST document the `--test-short` flag in the CI Integration Pattern section.

- GIVEN a user configuring Gaze in their CI pipeline
- WHEN they read the CI Integration Pattern section
- THEN the section documents `--test-short` as an optional flag with a migration callout explaining the coverage behavior change and a link to the Gaze migration notes

## MODIFIED Requirements

### Project Description and Framing

The Gaze project page description MUST be updated to reflect multi-language support. The description MUST NOT overstate maturity — Go is the primary supported language, and multi-language support is via the external analyzer protocol.

- GIVEN a user evaluating Gaze for a non-Go project
- WHEN they read the project description and lead text
- THEN the description explicitly states Go is the primary supported language and other languages are supported via the external analyzer protocol

### CLI Flags Table

The CLI flags table MUST be updated to include the three new flags.

- GIVEN a user looking up available CLI flags
- WHEN they view the CLI flags table
- THEN they see all current flags including `--analyzer <binary>` (crap/quality/report), `--language <lang>` (crap/quality/report), and `--test-short` (crap/report/self-check)

### Architecture Table

The architecture table MUST be updated to reflect any new packages added for multi-language support, if such packages exist in the upstream codebase. If no new packages exist, no update is needed. The table MUST remain accurate against the upstream codebase.

- GIVEN a developer reviewing Gaze's architecture
- WHEN they view the architecture package table
- THEN it accurately reflects the current package structure including any new packages for external analyzer support

### Homepage Card

The homepage Gaze card badge MUST be updated to reflect multi-language support.

- GIVEN a visitor browsing the Unbound Force homepage
- WHEN they see the Gaze project card
- THEN the badge and description reflect Gaze's expanded scope beyond Go-only analysis

### Tester Guide Side Effect Count Consistency

The tester guide's side effect count MUST be updated to be consistent with the expanded taxonomy on the project page.

- GIVEN a user reading both the tester guide and the project page
- WHEN they compare side effect counts
- THEN the numbers are consistent and not contradictory

### Related Pages Multi-Language Framing

The team page (`gaze-tester.md`) and projects index page (`_index.md`) MUST be updated to reflect multi-language framing consistent with the project page.

- GIVEN a user navigating from the projects index or team page to the Gaze project page
- WHEN they compare the Gaze descriptions
- THEN the framing is consistent with the updated project page (no "for Go" framing that contradicts multi-language support)

### Current Limitations Section Accuracy

The Current Limitations section MUST be reviewed and updated for accuracy against the expanded taxonomy.

- GIVEN a user reading the Current Limitations section
- WHEN they compare the stated limitations with the expanded taxonomy documented elsewhere on the page
- THEN all stated limitations remain accurate and are not contradicted by the new content (e.g., P3-P4 limitation is scoped to Go analysis, external analyzer scope is noted)

## REMOVED Requirements

None.
