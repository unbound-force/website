## Context

Upstream `unbound-force/unbound-force#269` changed the error reporting in `uf init` and `uf setup`. Previously, failures produced hardcoded summary strings. Now, `err` and `output` fields from sub-tool execution are surfaced directly, giving users the actual error message from the failing command.

The website's CLI reference (`content/docs/reference/cli.md`) and developer guide (`content/docs/getting-started/developer.md`) describe these commands but do not mention error reporting behavior. Two small text additions bring the docs in line with the shipped behavior.

## Goals / Non-Goals

### Goals

- Add error output behavior documentation to the `init` and `setup` sections of the CLI reference page
- Add an error output note to the Sub-Tool Initialization section of the developer guide
- Keep additions minimal — one sentence per location

### Non-Goals

- Creating a changelog or release notes infrastructure (no such infrastructure exists; tracked separately)
- Adding troubleshooting pages (no existing troubleshooting content references the old generic error messages — confirmed by triage search)
- Documenting specific error messages or failure modes (error output is tool-dependent and varies)

## Decisions

1. **CLI reference + developer guide only**: Triage confirmed zero references to old generic error messages across all website content. The only actionable work is adding notes about the new behavior to the two pages that describe `uf init` and `uf setup` in detail.

2. **No release notes entry**: The website has no changelog or release notes infrastructure. Creating one for a single UX improvement would violate Constitution Principle II (Minimal Footprint). The issue's request for a "release notes entry" is acknowledged but deferred until release notes infrastructure is established.

3. **Prose note, not a flags table entry**: Error output behavior is not a flag — it's default behavior. A sentence in the command description is the appropriate location.

## Risks / Trade-offs

- **Risk**: The error output format may change in future upstream releases, requiring another docs update. Accepted — the added text describes behavior generically ("displays the actual error output") without specifying format details.
- **Trade-off**: Not creating release notes infrastructure means the issue's full ask is not fulfilled. Accepted — the documentation update is the actionable portion; release notes is a separate feature.
