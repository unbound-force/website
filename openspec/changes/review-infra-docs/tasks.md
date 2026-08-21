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

## 1. New Reference Pages

- [x] 1.1 [P] Create convention packs reference page at `content/docs/reference/convention-packs.md` with `weight: 30`. Include: definition of convention packs, severity levels (MUST/SHOULD/MAY), loading mechanism (`.opencode/uf/packs/`), all available pack types (default, content, go, typescript, python, CI, severity) plus `-custom` variants, CI pack scope and rule count, cross-reference to constitution governance hierarchy. Source content from upstream `.opencode/uf/packs/` files and issue #206.
- [x] 1.2 [P] Create council-review-action reference page at `content/docs/reference/council-review-action.md` with `weight: 40`. Include: action purpose, three-workflow chain architecture (trigger → review → comment), persona discovery mechanism, configuration options (inputs, secrets, environment variables) with security note on least-privilege secret scoping, integration with review lifecycle. Source content from issue #190 and upstream action repository.

## 2. New Tutorial Page

- [x] 2.1 Create council-review-action tutorial at `content/docs/getting-started/council-review-action-tutorial.md` with `weight: 75`. Include: prerequisites, step-by-step workflow file setup, example YAML configuration using `${{ secrets.* }}` syntax (never placeholder tokens), verification steps (test PR), troubleshooting common issues, link to reference page. Frame the action as assisting human reviewers, not replacing them. Follow the pattern of existing code-review-tutorial.md. Source content from issue #192.

## 3. CLI Reference Updates

- [x] 3.1 Add an "OpenCode Slash Commands" top-level section to `content/docs/reference/cli.md` with an introductory paragraph explaining that slash commands are typed within an AI coding session (not a terminal). Then add `/agent-brief` as the first entry. Cover: command purpose (AGENTS.md lifecycle management), two modes (create/audit), usage syntax, example output. Source from issue #60.
- [x] 3.2 Add `/review-pr` section to `content/docs/reference/cli.md`. Cover: command purpose (post-PR GitHub review), CI causality analysis, difference from `/review-council`, usage syntax, cross-reference to code-review-tutorial.md. Source from issue #59. Note: runs after task 3.1 (same file).
- [x] 3.3 Add `/address-feedback` section to `content/docs/reference/cli.md`. Cover: command purpose (PR review feedback triage), four-phase workflow (collect, classify, plan, execute), usage syntax. Source from issue #138. Note: runs after task 3.2 (same file).

## 4. Existing Page Updates

- [x] 4.1 [P] Add Principle V (Security by Default) to `content/docs/getting-started/constitution.md`. Follow the structure of Principles I-IV: heading, description, MUST rules, SHOULD rules, rationale. Cover supply chain integrity (hash verification), input validation, least privilege, and dependency justification as defined in the upstream constitution. Do NOT include compound severity escalation (that belongs in the convention packs reference, not the constitution). Update frontmatter description from "4 core principles" to "5 core principles". Reconcile constitution version (website shows v1.1.0; verify upstream version). Also update the "Next Steps" section to link to the new convention packs reference page. Source from issue #142 and upstream `.specify/memory/constitution.md`.
- [x] 4.2 [P] Update branch naming in `content/docs/getting-started/common-workflows.md`. Replace all instances of `NNN-<name>` branch pattern with `speckit/NNN-<name>`. Update any example branch names (e.g., `013-binary-rename` → `speckit/013-binary-rename`). Source from issue #205.
- [x] 4.3 [P] Update developer guide at `content/docs/getting-started/developer.md`. (a) Add or expand convention pack section with cross-reference to the new convention packs reference page. Brief overview of how packs integrate with the review lifecycle. (b) Update branch naming from `NNN-<short-name>` to `speckit/NNN-<short-name>` (line 130 contains the old pattern). Note: task 4.2 handles common-workflows.md; this task handles developer.md.

- [x] 4.4 [P] Update reference section index page at `content/docs/reference/_index.md`. Add entries for the new Convention Packs and Council Review Action reference pages, following the existing pattern of section heading + bullet with link and description.

## 5. Verification

- [x] 5.1 Run `npm run build` to verify all new and modified pages build without errors.
- [x] 5.2 Run `npm run dev` and visually verify: new pages render correctly, sidebar navigation includes new pages, cross-references link to correct targets (click each internal link in new/modified pages to verify no 404s), dark mode renders correctly, all new pages have complete Hugo frontmatter (title, description, lead, date, weight, toc, draft: false). Note: runs after task 5.1.
- [x] 5.3 Verify Principle V content accuracy: compare the Principle V section on the constitution page against the upstream `.specify/memory/constitution.md` to confirm all MUST rules, SHOULD rules, and rationale accurately reflect the upstream source. Verify that compound severity escalation is NOT present under Principle V (it belongs in the convention packs reference). Note: runs after task 5.2.
- [x] 5.4 Verify constitution alignment: Principle I (new docs describe artifact-based patterns), Principle II (convention pack docs cover independent loading), Principle III (severity levels and quality enforcement documented), Principle IV (N/A, documentation-only), Principle V (N/A, documentation-only). Note: runs after task 5.3.

<!-- spec-review: passed -->
<!-- code-review: passed -->
