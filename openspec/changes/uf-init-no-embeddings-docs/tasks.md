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

## 1. Update Content Pages

Each task targets a different file — all are parallel-eligible.

- [x] 1.1 [P] Update `content/docs/getting-started/knowledge.md`: In the "What `uf init` Creates" section (~lines 107–111), change step 3 from `dewey index` to `dewey index --no-embeddings`. Add a brief note explaining that embeddings are deferred for faster initialization, and that users can run `dewey index` separately (without `--no-embeddings`) to generate embeddings. Reference the existing `--no-embeddings` entry in the Global CLI Flags table rather than duplicating the flag description.
- [x] 1.2 [P] Update `content/docs/getting-started/developer.md`: In the Sub-Tool Initialization section (~lines 291, 322), update the `--force` re-index description to note that `dewey index` now runs with `--no-embeddings`. Update any text that says "re-indexes an existing Dewey workspace" to clarify that re-indexing defers embedding generation.
- [x] 1.3 [P] Update `content/docs/reference/cli.md`: Update the `--force` flag description (~line 41) to note that re-indexing uses `--no-embeddings`, deferring embedding generation.
- [x] 1.4 [P] Update `content/docs/getting-started/common-workflows.md`: Update the setup workflow section (~line 436) where it mentions `uf init` running `dewey init` + `dewey index` to note the `--no-embeddings` default.
- [x] 1.5 [P] Review `content/blog/dewey-knowledge-retrieval.md` (~line 98): Check whether the `uf init` mention makes a factually incorrect claim given the `--no-embeddings` change. Update only if the text would mislead a reader; leave unchanged if the mention is general enough to remain accurate (per design decision D4).

## 2. Verification

- [x] 2.1 Run `npm run build` and verify no build errors.
- [x] 2.2 Grep all content files for `uf init` and `dewey index` to confirm no stale references were missed in tasks 1.1–1.5.
- [x] 2.3 Verify constitution alignment: Content Accuracy (Principle I) — all updated pages reflect the actual `--no-embeddings` behavior. Minimal Footprint (Principle II) — only in-place edits, no new sections or dependencies. Visitor Clarity (Principle III) — updated text sets correct expectations about semantic search availability after init.

<!-- spec-review: passed -->
