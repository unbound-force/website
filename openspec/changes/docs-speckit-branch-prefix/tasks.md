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

## 1. Inventory Documentation References

- [x] 1.1 Search the repository for `NNN-<short-name>`, `NNN-feature-name`, and related Speckit branch wording; classify matches as current guidance, legitimate numbered paths, or historical context.
- [x] 1.2 Identify every current user-facing or contributor-facing reference that must use `speckit/NNN-<short-name>`, including any references beyond the known `AGENTS.md`, contributing page, and pipeline article.

## 2. Update Independent Documentation Files

- [x] 2.1 [P] Update `AGENTS.md` branch conventions and examples to use `speckit/NNN-<short-name>`, explain that existing branches are not renamed, and preserve the `opsx/<name>` OpenSpec convention.
- [x] 2.2 [P] Update `content/docs/contributing/_index.md` contributor branch setup guidance to use the prefixed Speckit format and compatibility wording.
- [x] 2.3 [P] Update `content/blog/the-8-phase-pipeline.md` current branch naming guidance and example while preserving the article's historical or explanatory context where applicable.
- [x] 2.4 [P] Update additional current guidance discovered during task 1.2 in `content/docs/getting-started/developer.md`, `content/docs/getting-started/common-workflows.md`, `.opencode/commands/uf.agent-brief.md`, `.opencode/commands/uf.unleash.md`, `.opencode/commands/speckit.specify.md`, `.opencode/commands/uf.finale.md`, `.specify/templates/spec-template.md`, and `.specify/templates/plan-template.md`; keep explicit legacy compatibility notes. Runtime validation in `.specify/scripts/bash` is handled in section 4.

## 3. Validate the Documentation Change

- [x] 3.1 Re-run targeted searches to confirm current Speckit branch guidance uses `speckit/NNN-<short-name>`, obsolete current-format examples are gone, and `opsx/<name>` references remain unchanged.
- [x] 3.2 Run `npm run build` and confirm the Hugo production build succeeds without broken-content or rendering errors.
- [x] 3.3 Verify constitution alignment: the final documentation remains artifact-based, introduces no mandatory dependencies, exposes the convention through deterministic searchable content, and is independently verifiable without external services.

## 4. Synchronize Speckit Branch Validation

- [x] 4.1 Backport `speckit/` prefix stripping into `.specify/scripts/bash/common.sh` so `speckit/NNN-<name>` branches resolve to `specs/NNN-<name>/`.
- [x] 4.2 Update `.specify/scripts/bash/create-new-feature.sh` to emit `speckit/NNN-<name>` branch names while keeping the feature directory unprefixed.
- [x] 4.3 Smoke-test `check-prerequisites.sh` and `create-new-feature.sh` against a `speckit/NNN-<name>` branch.
- [x] 4.4 Clarify the `--paths-only` help text in `.specify/scripts/bash/check-prerequisites.sh` to state that plan/tasks checks are skipped while branch validation still runs.

<!-- spec-review: passed -->
