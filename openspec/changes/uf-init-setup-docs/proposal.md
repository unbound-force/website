## Why

The `uf init` and `uf setup` commands are the entry point to the entire Unbound Force toolchain. Today, users encounter these commands in scattered mentions across Quick Start, Developer Guide, and Common Workflows pages -- but there is no dedicated documentation that explains the full lifecycle: what `uf setup` installs, what `uf init` scaffolds, how `--force` re-initialization works, idempotency guarantees, sub-tool initialization order, and troubleshooting.

Eight resolved GitHub issues (#203, #210, #211, #222, #223, #225, #236, #245) document significant changes to these commands since launch -- concurrent sub-tool initialization, `--force` behavior for all sub-tool groups, independent RPM version resolution, command-specific guardrails, and stale command reference warnings. None of this operational knowledge has been documented on the website.

Users who hit issues during setup or init (e.g., Dewey indexing hangs, `--force` not re-initializing Group B tools, stale command references after a version upgrade) have no documentation to reference. This creates support burden and slows onboarding.

## What Changes

### New content on the existing Common Workflows page

Add a dedicated **`uf init` / `uf setup` Lifecycle** section to the Common Workflows page that covers:

- **`uf setup` install cascade**: What tools get installed, in what order, how platform detection works (Homebrew vs dnf vs curl fallback), and what `--dry-run` does.
- **`uf init` scaffolding lifecycle**: The 12-step initialization process -- what each step does, what files it creates/modifies, and how idempotency works on re-runs.
- **Sub-tool initialization**: Concurrent initialization of Dewey, Gaze, Replicator, Specify, and OpenSpec -- what each does, Group A vs Group B classification, and how `--force` affects each group.
- **Re-initialization (`--force`)**: When to use it, what it does for each sub-tool group, the "re-initialized" vs "initialized" output distinction.
- **Guardrail injection**: Command-specific guardrails (speckit.implement, speckit.constitution, speckit.taskstoissues, execution/utility) and how self-correction works on re-runs.
- **Troubleshooting**: Common issues (Dewey hang on `--force`, stale command references, RPM version resolution failures on Fedora/RHEL) with solutions.

### Updates to existing pages

- **Quick Start**: Add a cross-reference link to the new lifecycle section for users who want deeper understanding.
- **Developer Guide**: Add a cross-reference link from the Prerequisites section to the lifecycle section.

## Capabilities

### New Capabilities
- `uf-init-lifecycle-docs`: Dedicated documentation section covering the full `uf init` / `uf setup` lifecycle with troubleshooting guidance.

### Modified Capabilities
- `quick-start-page`: Gains a cross-reference to the lifecycle section for deeper reading.
- `developer-guide-page`: Gains a cross-reference from the Prerequisites section.

### Removed Capabilities
- None.

## Impact

- **Files modified**: `content/docs/getting-started/common-workflows.md` (new section), `content/docs/getting-started/quick-start.md` (cross-reference), `content/docs/getting-started/developer.md` (cross-reference).
- **Navigation**: No menu changes needed -- the new content lives within an existing page.
- **Build**: No template, SCSS, or configuration changes. Content-only.

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

This change is documentation-only. It does not modify artifact formats, hero communication patterns, or runtime coupling between tools.

### II. Composability First

**Assessment**: PASS

The documentation describes `uf init` and `uf setup` as composable operations -- each sub-tool is independently initializable, `uf init` is idempotent, and the `--force` flag works per-group. The documentation reinforces the composability principle by explaining how tools function independently.

### III. Observable Quality

**Assessment**: N/A

This change does not produce machine-parseable output or modify provenance metadata. It documents tools that already satisfy this principle.

### IV. Testability

**Assessment**: N/A

This change is static Markdown content. Validation is `npm run build` (build succeeds) and visual review (content renders correctly in both light and dark mode).

### V. Security by Default

**Assessment**: N/A

This change is static Markdown content. No dependencies, external inputs, or file permissions are affected.
