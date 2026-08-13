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

## 1. Verify upstream behavior

- [x] 1.1 Confirm the actual `specify init` invocation flags by reading the upstream change (unbound-force/unbound-force#216) or inspecting `uf init` source. Verify `--here`, `--integration opencode`, `--offline` are the correct flags and understand skip-if-exists behavior.

## 2. Update documentation pages

- [x] 2.1 [P] Update `content/docs/getting-started/developer.md` — Add a specify initialization bullet to the Sub-Tool Initialization section (after line 322, between Dewey init and opencode.json config). Describe: creates `.specify/` with Speckit configuration, operates offline without network calls, targets OpenCode integration, skipped if `.specify/` already exists. Follow the style and detail level of existing bullets in that section.

- [x] 2.2 [P] Update `content/docs/reference/cli.md` — Update the `init` description paragraph (line 30) to mention that `uf init` also initializes the specify sub-tool (creates `.specify/` directory). Do NOT add flags to the flag table — `--here`, `--integration`, `--offline` are internal arguments. Keep the description concise and consistent with the existing style.

- [x] 2.3 [P] Update `content/docs/getting-started/common-workflows.md` — Append specify initialization to the sub-tool initialization summary sentence (line 436). Add it alongside the existing Dewey and opencode.json mentions, maintaining the same compact summary style.

## 3. Verify and validate

- [x] 3.1 Run `npm run build` and confirm zero errors
- [x] 3.2 Run `npm run dev` and visually verify that all three updated pages render correctly — check that the new specify initialization content appears in the right location, formatting is correct, and no existing content is disrupted
- [x] 3.3 Verify constitution alignment: Content Accuracy (Principle I) — confirm the documented behavior matches the actual upstream `uf init` specify initialization behavior. Minimal Footprint (Principle II) — confirm no new pages, templates, or dependencies were added. Visitor Clarity (Principle III) — confirm the content is framed at the behavioral level, not as internal CLI flags.

<!-- spec-review: passed -->
