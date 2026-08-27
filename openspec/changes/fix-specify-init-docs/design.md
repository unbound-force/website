## Context

`uf init` now invokes `specify init --here --integration opencode --offline` instead of bare `specify init` as part of its sub-tool initialization phase. The website documentation currently omits specify initialization entirely from the Sub-Tool Initialization section in `developer.md` and from the CLI reference in `cli.md`.

This change updates 3 Markdown files to add specify initialization as a documented sub-tool step. Per the proposal, the constitutional alignment is PASS on all three website principles (Content Accuracy, Minimal Footprint, Visitor Clarity).

## Goals / Non-Goals

### Goals

- Document that `uf init` performs specify initialization as a sub-tool step
- Describe the behavioral model: offline (no network calls), directory-scoped (`.specify/` created in current directory), OpenCode-targeted
- Maintain consistent abstraction level across all three affected pages
- Frame the change from the user's perspective, not as internal implementation detail

### Non-Goals

- Documenting `specify init` as a standalone user-facing command (users run `uf init`, not `specify init`)
- Adding troubleshooting content for `.specify/` creation failures (out of scope unless a natural fit exists in the existing structure)
- Creating new pages or sections — all changes fit within existing content structure
- Adding automated drift detection between upstream CLI and website docs (systemic gap, separate initiative)

## Decisions

### D1: Abstraction level — behavioral, not flags

**Decision**: Describe what `uf init` does (creates `.specify/` directory with Speckit configuration, operates offline without network calls) rather than the exact internal invocation (`specify init --here --integration opencode --offline`).

**Rationale**: The website serves users who run `uf init`. Exposing internal subprocess flags would violate Visitor Clarity (Principle III) — users might attempt to run `specify init` directly and bypass `uf init`'s additional scaffolding. The behavioral description is accurate without being leaky.

**Exception**: The `developer.md` Sub-Tool Initialization section MAY mention the specify integration target (OpenCode) as context for what the `.specify/` directory configures, since that section already describes sub-tool implementation details (e.g., "runs `dewey init`").

### D2: Insertion point — after Dewey, before opencode.json

**Decision**: Add the specify initialization bullet to `developer.md` Sub-Tool Initialization (line 322) between the Dewey initialization bullet and the opencode.json configuration bullet. This matches the actual execution order in the `uf init` code.

**Rationale**: The Sub-Tool Initialization section lists steps in execution order. Specify initialization runs after Dewey init but before opencode.json configuration in the upstream implementation.

### D3: CLI reference — description update, not flag addition

**Decision**: Update the `cli.md` init description paragraph (line 30) to mention specify initialization. Do NOT add `specify`-related flags to the flag table — there are none exposed to the user.

**Rationale**: `--here`, `--integration`, and `--offline` are not user-facing `uf init` flags. They are internal arguments passed to the `specify` subprocess.

### D4: common-workflows.md — append to existing summary

**Decision**: Append specify initialization to the existing sub-tool summary sentence on line 436, maintaining the same style as the existing Dewey and opencode.json mentions.

**Rationale**: This line is a compact summary of what `uf init` does during setup. Adding specify initialization keeps it complete without restructuring.

## Risks / Trade-offs

### R1: Upstream behavior may change again

**Risk**: The `specify init` invocation may change in future upstream releases. The website documentation will become stale again.

**Mitigation**: This is a known systemic gap (no automated drift detection). The Website Documentation Gate in AGENTS.md requires upstream repos to file website issues when user-facing behavior changes. This issue (#214) is evidence the gate is working — the risk is that future changes are missed.

### R2: Developer.md Sub-Tool Initialization section grows long

**Risk**: Adding another bullet point to an already detailed section may reduce scannability.

**Mitigation**: The specify initialization step is a single bullet with a brief description, consistent with the existing Dewey and opencode.json bullets. The section remains within a reasonable length.
