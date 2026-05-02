## 1. Fix `/finale` merge description (#53)

- [x] 1.1 Update `content/docs/getting-started/common-workflows.md`: Remove "Merge PR" step from the `/finale` workflow table, renumber remaining steps, update guardrails to say "Never merges the PR", update description to say "/finale wraps up the branch and creates a PR for review"
- [x] 1.2 Update `content/docs/getting-started/quick-start.md`: Change **both** `/finale # ship it` comments (Large Tasks and Small Tasks code blocks) to `/finale # commit, push, create PR`
- [x] 1.3 Update `content/docs/getting-started/developer.md`: Fix **all four** `/finale` merge references: (a) daily workflow description, (b) Session Lifecycle table "End" row, (c) Session Ritual prose, (d) Session Ritual code block comment — remove "merge" and "rebase-merge" language, replace with "create a PR"
- [x] 1.4 Update `content/blog/unleash-in-practice.md`: Fix The Complete Loop section — remove "rebase-merge" from prose (line 135) and change code block comment from "merge → main" to "PR → main" (line 139)

## 2. Fix `/unleash` branch support (#54)

- [x] 2.1 Update `content/docs/getting-started/common-workflows.md`: Replace text stating `/unleash` rejects `opsx/*` branches with text stating it supports both `NNN-*` and `opsx/*` branches, including OpenSpec task detection behavior

## 3. Fix `uf init` file count and breakdown (#55)

- [x] 3.1 Verify current file counts against upstream `unbound-force/unbound-force/.opencode/command/` and `.opencode/agents/` directories before writing
- [x] 3.2 Update `content/docs/getting-started/developer.md`: Change "approximately 50 files" to the verified count (~35), update the breakdown table to reflect current command and agent counts (verify against upstream at implementation time)

## 4. Fix `mxf` install instruction (#56)

- [x] 4.1 Update `content/docs/getting-started/product-manager.md`: Replace the stale `brew install unbound-force/tap/unbound-force` instruction with `brew install unbound-force/tap/mxf` and add `uf setup` as an alternative

## 5. Verification

- [x] 5.1 Run `npm run build` and confirm no build errors
- [ ] 5.2 Run `npm run dev` and visually verify all corrected pages render correctly
- [x] 5.3 Search all files under `content/` for remaining references to `/finale` merging PRs or `/unleash` rejecting `opsx/*` branches (colloquial uses of "ship" that don't imply PR merging are acceptable)
<!-- spec-review: passed -->
<!-- code-review: passed -->
