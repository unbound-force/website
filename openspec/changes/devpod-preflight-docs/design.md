## Context

PR unbound-force/unbound-force#436 removed the `LookPath("podman")` pre-flight check from `uf sandbox create --backend devpod` and added a diagnostic hint on `devpod up` failure. The website documentation currently states Podman must be installed as a prerequisite and does not mention the diagnostic hint. These docs need updating to match the new behavior.

The proposal (constitution alignment: all N/A) confirms this is a documentation-only change with no hero functionality, artifact interfaces, or testability implications.

## Goals / Non-Goals

### Goals
- Update sandbox reference prerequisites to remove standalone Podman binary requirement for DevPod workspaces
- Document the new `devpod up` failure diagnostic hint (`uf doctor` / `uf setup`)
- Clarify that `uf setup` registers a docker-type provider under the name `podman` via `DOCKER_PATH=podman`
- Update blog post to remove misleading "Podman required" limitation for DevPod users
- Add changelog entry for the pre-flight change
- Preserve accuracy: ephemeral Podman sandbox (`uf sandbox start` without DevPod) still requires Podman installed

### Non-Goals
- Rewriting the sandbox reference page structure or adding new sections
- Documenting the upstream code changes (that belongs in the unbound-force repo)
- Updating DevPod provider internals or architecture diagrams
- Modifying CLI reference (subcommand table is already accurate)

## Decisions

### D1: Distinguish ephemeral vs. persistent prerequisites

The sandbox has two paths: ephemeral containers (`uf sandbox start` — uses Podman directly) and persistent workspaces (`uf sandbox create` — uses DevPod). Only the DevPod path dropped the standalone Podman requirement. The documentation must make this distinction clear to avoid confusing users who use ephemeral containers (and still need Podman).

**Approach**: Split the prerequisites paragraph into two contexts — ephemeral (Podman required) and persistent/DevPod (`uf setup` configures everything). This avoids a blanket statement in either direction.

### D2: Blog post update scope

The blog post `sandbox-isolation.md` lists "Podman required" as a current limitation. Rather than removing the limitation entirely (since ephemeral mode still needs it), update the wording to clarify the limitation applies to ephemeral containers only, and that DevPod workspaces handle runtime resolution automatically.

### D3: Changelog placement

Add the pre-flight change to the changelog as a "Changed" item under the next release version. Since the upstream PR targets a future release, use a placeholder version header that will be finalized at release time.

## Risks / Trade-offs

### Risk: Over-simplification
Removing "Podman required" without nuance could lead DevPod users to think they never need Podman, when ephemeral mode still requires it. **Mitigation**: Explicit split between ephemeral and DevPod prerequisites.

### Risk: Blog post drift
Blog posts are point-in-time artifacts. Editing post-publication content could create confusion if readers saw the original version. **Mitigation**: The edit is factual correction, not opinion change. The limitation is being refined, not removed.
