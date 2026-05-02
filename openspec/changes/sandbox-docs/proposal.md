## Why

The `uf sandbox` command is one of the most significant features in the Unbound Force CLI — providing containerized, isolated AI agent sessions via Podman. It has 7 subcommands (`start`, `stop`, `attach`, `extract`, `status`, `create`, `destroy`), two mount modes, UID mapping, persistent workspaces, and a CDE backend. None of this is documented on the website.

This change addresses GitHub issues #39 (basic sandbox docs), #47 (UID mapping requirements), and #61 (persistent workspaces and CDE backend).

## What Changes

1. **Sandbox documentation page**: New page at `content/docs/reference/sandbox.md` (or `content/docs/getting-started/sandbox.md`) covering the full `uf sandbox` command surface — all 7 subcommands, mount modes, security model, and platform-specific setup.

2. **UID mapping section**: Prerequisites for UID mapping, macOS Podman machine configuration, `--uidmap` escape hatch, Podman >= 4.3 requirement.

3. **Persistent workspaces section**: `create`/`destroy` commands, named Podman volumes, workspace lifecycle, bidirectional git sync.

4. **CDE backend section** (experimental): Eclipse Che / Dev Spaces backend, `--backend` flag, backend resolution matrix. Labeled as experimental since end-to-end validation is incomplete.

## Capabilities

### New Capabilities
- `docs/reference/sandbox`: Comprehensive sandbox documentation page covering all subcommands, modes, and platform setup
- UID mapping prerequisites and troubleshooting section
- Persistent workspace lifecycle documentation
- CDE backend documentation (experimental label)

### Modified Capabilities
- CLI reference page (from `cli-reference-and-positioning` change): sandbox entry links to detailed docs

### Removed Capabilities
- None

## Impact

- 1 new content page
- Cross-references to CLI reference page (dependency on `cli-reference-and-positioning` change)
- Platform-specific setup instructions (macOS, Fedora/RHEL)

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

Documentation page — no effect on hero communication.

### II. Composability First

**Assessment**: PASS

Documents `uf sandbox` as a standalone capability. The sandbox works independently of other tools (OpenCode runs inside, but sandbox management is self-contained).

### III. Observable Quality

**Assessment**: PASS

Documents the `uf sandbox status` command which provides observable state, and the security model which documents the isolation guarantees.

### IV. Testability

**Assessment**: N/A

No code changes. Validation is `npm run build` + visual review.
