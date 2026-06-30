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

## 1. Update Getting Started Guide (knowledge.md)

- [x] 1.1 Add `DEWEY_CHUNK_MAX_CHARS` to the Embedding Model Alignment section in `content/docs/getting-started/knowledge.md`. Add the env var to the shell profile export block with default value `12288` and a description explaining it overrides the configured max chunk characters for embedding.

- [x] 1.2 Update the `dewey doctor` diagnostic table in `content/docs/getting-started/knowledge.md`. Extend the Embedding Layer row description to mention legacy model advisory detection alongside Ollama availability and model status.

- [x] 1.3 Add a note below the `dewey doctor` table in `content/docs/getting-started/knowledge.md` explaining the legacy model advisory: when `granite-embedding:30m` is configured, `dewey doctor` displays an informational note suggesting an upgrade to Granite Embedding R2 when it becomes available.

## 2. Update Team Page (dewey.md)

- [x] 2.1 Extend the `embedding:` YAML config example in the Embedding Model section of `content/docs/team/dewey.md` (around line 129) to include `max_chunk_chars: 12288` with a comment explaining the field controls the maximum chunk size for embedding.

- [x] 2.2 Add a note to the Embedding Model section in `content/docs/team/dewey.md` mentioning the Granite Embedding R2 model as a future upgrade path. Use forward-looking language -- do not present R2 as currently available or as an action item.

## 3. Update Project Page (dewey.md)

- [x] 3.1 [P] Add a note to the Semantic Search subsection in `content/docs/projects/dewey.md` (around line 43) mentioning chunk size configurability via `DEWEY_CHUNK_MAX_CHARS` or `max_chunk_chars` config, and noting the Granite Embedding R2 model as a planned future default.

## 4. Verification

- [x] 4.1 Run `npm run build` and verify no build errors.

- [x] 4.2 Run `npm run dev` and visually verify the three updated pages render correctly with all new content visible and properly formatted.

- [x] 4.3 Verify consistency across all three pages -- ensure env var names, default values, and R2 language are identical wherever they appear.
<!-- spec-review: passed -->
<!-- code-review: passed -->
