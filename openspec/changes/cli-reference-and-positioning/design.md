## Context

The `uf` CLI is the primary user interface for Unbound Force — it installs, configures, and manages OpenCode, Speckit, and Replicator. Despite having 6 command groups and 15+ subcommands, the website only documents `init`, `setup`, and `doctor` scattered across getting-started pages. There is no single reference page and no changelog.

## Goals / Non-Goals

### Goals
- Create a comprehensive CLI reference page listing all commands, subcommands, and flags
- Add `uf` as a layer in the stack positioning table
- Create a changelog page for tracking release-level changes
- Establish a reference section in the docs navigation

### Non-Goals
- Detailed tutorials for each command (covered by separate tutorial specs)
- Exhaustive flag documentation for `sandbox` and `gateway` (covered by their own doc specs)
- Backfilling changelog entries for historical releases (include the current release only)

## Decisions

1. **CLI reference as a table-driven page**: Use tables for command/flag reference rather than prose. Tables are scannable and maintainable. Each command group gets a section with a subcommand table.

2. **Stack table adds CLI as the top layer**: The `uf` CLI sits above the existing three tools because it's the entry point that bootstraps and manages them. Position: CLI > Agent > Planning > Coordination.

3. **Changelog in docs section, not blog**: The changelog is a reference page, not a narrative. Place it under `docs/changelog/` with weight ordering so newest entries appear first.

4. **Reference section in navigation**: Create a new "Reference" section in the docs sidebar to house the CLI reference and future reference pages (e.g., configuration reference).

5. **Source command data from upstream**: Pull command details from `uf --help` and `uf <command> --help` output, cross-referencing with the `unbound-force/unbound-force` repo's CLI source code. `--help` output is the authoritative source. Cross-reference with Dewey for any curated content. Include a version marker (e.g., "This page reflects `uf` vX.Y.Z") so staleness is detectable.

## Risks / Trade-offs

- **Risk**: CLI surface may change between spec creation and implementation. Mitigated by sourcing from the shipped binary at implementation time.
- **Risk**: Changelog page requires ongoing maintenance with each release. Mitigated: changelog updates are part of the release process; the Website Documentation Gate in AGENTS.md already requires issues for user-facing changes. If the changelog falls more than 2 releases behind, remove it rather than show stale content (per Constitution I: Content Accuracy).
- **Trade-off**: Brief descriptions in the CLI reference vs. detailed docs. Chose brief — the reference links to detailed pages where they exist.
