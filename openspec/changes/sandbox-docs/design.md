## Context

The `uf sandbox` command surface spans 7 subcommands across three capability tiers: basic session management (start/stop/attach/extract/status), persistent workspaces (create/destroy), and an experimental CDE backend (Che). Three separate issues (#39, #47, #61) each cover a piece of this surface.

## Goals / Non-Goals

### Goals
- Create a single, comprehensive sandbox documentation page covering all three capability tiers
- Include platform-specific setup instructions (macOS Podman machine, Fedora SELinux)
- Document the security model (rootless Podman, read-only mounts, no push credentials)
- Include UID mapping prerequisites and troubleshooting
- Label CDE backend as experimental with appropriate caveats

### Non-Goals
- Tutorial-style walkthrough (covered by blog post #40)
- Container image customization guide (separate concern, references containerfile repo)
- Gateway integration details (covered by gateway-docs change)

## Decisions

1. **Single page, not a section**: All sandbox docs on one page. The content is interconnected (persistent workspaces extend basic sessions, UID mapping applies to both). A single page is easier to navigate than a multi-page section for this scope.

2. **Page location**: Place under `content/docs/reference/sandbox.md` alongside the CLI reference. Sandbox is a reference topic, not a getting-started guide.

3. **Three-tier structure**: Organize the page into: (a) Basic session management, (b) Persistent workspaces, (c) CDE backend. This mirrors the capability tiers and makes the experimental status of the CDE section clear.

4. **Security model as a table**: Use a table format for the security properties (rootless, read-only mounts, resource limits, etc.) — scannable and precise.

5. **Experimental label for CDE**: Use a Doks alert/callout to flag the CDE backend section as experimental. Document what works (API, mocked tests) and what hasn't been validated (end-to-end with production Che).

## Risks / Trade-offs

- **Risk**: Sandbox subcommands may evolve before this page ships. Mitigated by sourcing from the shipped binary at implementation time.
- **Trade-off**: Single page may become long. Accepted for now — the interconnected nature of sandbox features benefits from co-location. Can be split later if it exceeds ~500 lines.
