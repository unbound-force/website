<!--
  [P] marks tasks eligible for parallel execution.
  Add [P] when a task: (a) touches different files from
  other [P] tasks in the group, (b) has no dependency
  on prior tasks in the group, (c) can safely execute
  without ordering constraints.
  Do NOT add [P] when tasks modify the same file —
  parallel workers will cause merge conflicts.
  Tasks without [P] run sequentially first, then [P]
  tasks run in parallel.
-->

## 1. Add Lifecycle Section to Common Workflows

- [x] 1.1 Add a "uf setup / uf init Lifecycle" section to `content/docs/getting-started/common-workflows.md` after the existing "Environment Setup" section's `### 4. Start Working` subsection and before `## Next Steps`. The section MUST include:
  - **`uf setup` Install Cascade**: What tools get installed, the four categories (Core tools, Development tools, Knowledge layer, Project scaffolding), platform detection behavior (Homebrew vs dnf vs curl fallback), and the `--dry-run` flag. Source: Quick Start lines 22-29, issues #236.
  - **`uf init` Scaffolding**: What `uf init` does as the final step of setup -- creates `.uf/config.yaml`, deploys agents/commands/convention packs/templates, and performs sub-tool initialization. Mention the 12-step process at a conceptual level (not step-by-step implementation): file scaffolding, guardrail injection, STOP HERE blocks, scaffold comment deduplication, legacy directory cleanup. Source: issues #222, #223.
  - **Sub-tool Initialization**: Concurrent initialization of Dewey, Gaze, Replicator, Specify, and OpenSpec via independent goroutines. Group A (Dewey) vs Group B (Specify, Replicator, OpenSpec, Gaze) classification. Note that Dewey indexing no longer blocks other tools. Source: issue #203, #245.
  - **Re-initialization (`--force`)**: What `--force` does -- re-initializes ALL sub-tools (both Group A and Group B). Tools supporting `--force` receive the flag. Init summary shows "re-initialized" vs "initialized". Previously only Group A was re-initialized; this was fixed in v0.16.0. Source: issues #211, #245.
  - **Guardrail Injection**: Command-specific guardrails (4 variants: speckit.implement, speckit.constitution, speckit.taskstoissues, execution/utility). Self-correction on re-runs via correctness markers. Source: issue #223.
  - **Stale Command Warnings**: After `uf` namespace migration, `uf init` scans agent markdown files for references to old-name commands and warns with actionable output. Source: issue #225.

- [x] 1.2 Add a "Troubleshooting" subsection within the lifecycle section covering:
  - **Dewey indexing hangs on `--force`**: `--force` passes `--no-embeddings` to Dewey. Run `dewey index` separately for full embedding generation. Source: issue #211.
  - **Stale command references after upgrade**: `uf init` warns about old-name commands. Update agent markdown files to use `uf.*` namespace. Source: issue #225.
  - **RPM 404 errors on Fedora/RHEL**: Companion tool versions resolved independently via `gh release view`. Verify `gh` CLI is installed and has network access. Source: issue #236.

## 2. Add Cross-References

- [x] 2.1 [P] Add an inline cross-reference link in `content/docs/getting-started/quick-start.md` within the `uf setup` section (around line 22-29) pointing to the new lifecycle section in Common Workflows. Use the pattern: "For a detailed breakdown of what setup and init do under the hood, see [uf setup / uf init Lifecycle](/docs/getting-started/common-workflows/#uf-setup--uf-init-lifecycle)."

- [x] 2.2 [P] Add an inline cross-reference link in `content/docs/getting-started/developer.md` within the Prerequisites section (around line 14-26) pointing to the new lifecycle section. Use the same link pattern.

## 3. Verify

- [x] 3.1 Run `npm run build` and confirm no build errors.
- [x] 3.2 Run `npm run dev` and visually verify:
  - The new lifecycle section renders correctly on the Common Workflows page
  - The table of contents includes the new section heading
  - The cross-reference links from Quick Start and Developer Guide resolve correctly
  - Both light and dark mode render correctly
- [x] 3.3 Verify constitution alignment: confirm that no changes introduce runtime coupling, mandatory dependencies, or non-composable behavior (all PASS/N/A per proposal).
<!-- spec-review: passed -->
<!-- code-review: passed -->
