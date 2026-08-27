## Why

PR unbound-force/unbound-force#436 (fixing #431) changed the DevPod sandbox pre-flight behavior:

1. **Removed** the `LookPath("podman")` pre-flight check — users no longer need the `podman` binary in `$PATH`. The docker provider aliased as `podman` (configured by `uf setup`) handles container runtime resolution internally.
2. **Added** a diagnostic hint on `devpod up` failure: `"run 'uf doctor' to diagnose or 'uf setup' to configure"`.
3. **Unchanged**: The `--provider podman` flag is retained — it references the registered DevPod provider name, not a standalone binary.

The website documentation currently states Podman must be installed as a prerequisite and implies a standalone `podman` binary is required in `$PATH`. This is no longer accurate and could confuse users who follow the docs and install standalone Podman when `uf setup` already configures everything needed.

## What Changes

Update documentation across multiple pages to reflect the new DevPod provider model and diagnostic hint.

## Capabilities

### New Capabilities
- `diagnostic-hint-docs`: Document the new `devpod up` failure diagnostic hint that directs users to `uf doctor` and `uf setup`

### Modified Capabilities
- `sandbox-prerequisites`: Update prerequisites to clarify that `uf setup` configures the DevPod provider (a docker-type provider registered under the name `podman` via `DOCKER_PATH=podman`) — users do not need to install standalone Podman separately for DevPod workspaces
- `sandbox-blog-post`: Update the blog post to remove the implication that standalone Podman installation is a user prerequisite for DevPod workspaces

### Removed Capabilities
- `podman-in-path-prerequisite`: Remove documentation stating users need `podman` in `$PATH` as a prerequisite for DevPod sandbox usage

## Impact

**Affected pages** (files containing outdated podman prerequisite or DevPod provider information):

| File | What Needs Changing |
|------|-------------------|
| `content/docs/reference/sandbox.md` | Update Prerequisites paragraph (line 15): remove "Podman and DevPod must be installed" framing, clarify provider model, add diagnostic hint |
| `content/blog/sandbox-isolation.md` | Update "Current Limitations" section (line 147): "Podman required" is misleading for DevPod users; update blog post startup sequence to reflect new pre-flight behavior |
| `content/docs/reference/cli.md` | No changes needed — sandbox subcommand table is accurate as-is |
| `content/docs/getting-started/quick-start.md` | No changes needed — no Podman prerequisite mentioned |
| `content/docs/changelog/_index.md` | Add changelog entry for the pre-flight change in the next release section |

**What is NOT changing:**

- Ephemeral Podman sandbox (`uf sandbox start` without DevPod) still requires Podman — only the DevPod workspace path (`uf sandbox create`) dropped the standalone binary requirement
- `--backend podman` flag name is unchanged
- UID mapping, mount modes, security model — all unchanged

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

This is a documentation-only change. No artifact interfaces, hero communication protocols, or runtime coupling are affected.

### II. Composability First

**Assessment**: N/A

No hero functionality is added, removed, or modified. The change updates text content to reflect upstream CLI behavior changes.

### III. Observable Quality

**Assessment**: N/A

No machine-parseable output or provenance metadata is involved. This is a static website content update.

### IV. Testability

**Assessment**: N/A

Documentation changes are validated through `npm run build` (build succeeds) and visual verification (content renders correctly). No code-level testability concerns.
