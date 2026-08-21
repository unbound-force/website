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

## 1. Update Project Page (`content/docs/projects/dewey.md`)

All tasks in this group modify the same file. No [P] markers — execute sequentially.

- [x] 1.1 Update tool count from 48 to 50 in all locations on the page (paragraph text, section heading, section body). Refs: #41, #113
- [x] 1.2 Add RPM to the installation section (after Homebrew, before `go install`). Ref: #186
- [x] 1.3 Add curated knowledge stores to the key features list. Ref: #41
- [x] 1.4 Add pluggable providers (Ollama + Vertex AI) to the key features list. Ref: #113
- [x] 1.5 Add content sanitization to the key features list. Ref: #131

## 2. Update Knowledge Page (`content/docs/getting-started/knowledge.md`)

All tasks in this group modify the same file. No [P] markers — execute sequentially.

- [x] 2.1 Add RPM installation subsection between Homebrew and `go install`. Ref: #186
- [x] 2.2 Add note to Homebrew section that macOS cask install issues are fixed. Ref: #213
- [x] 2.3 Add `DEWEY_CHUNK_MAX_CHARS` to the environment variables reference table with description, config field (`embedding.max_chunk_chars`), and default value. Ref: #208
- [x] 2.4 Add `DEWEY_SYNTHESIS_ENDPOINT` to the env var reference with fallback chain and a callout about inverted precedence (config.yaml > env var for synthesis, opposite of embedding). Ref: #243
- [x] 2.5 Add `DEWEY_AUTHOR` to the env var reference with description (author tag for learning identities). Ref: #132
- [x] 2.6 Add provider configuration section with separate Ollama and Vertex AI config.yaml examples. Include `embedding.provider`, `synthesis.provider`, `gcloud` auth requirements, and `region: global` endpoint behavior. Refs: #113, #240
- [x] 2.7 Document global config path (`~/.config/dewey/config.yaml`) and per-vault override behavior. Ref: #113
- [x] 2.8 Update `dewey doctor` reference to include the Synthesis Layer section (provider type, endpoint, model, connectivity, Ollama model availability). Update diagnostic section count from 7 to 8. Ref: #249
- [x] 2.9 Add Vertex AI curation defaults note (16000 max output tokens, 300s timeout) to `dewey curate` or curation-related documentation. Ref: #209
- [x] 2.10 Add content sanitization subsection: 4-layer pipeline, per-source `sanitize_mode`/`trust_tier` in `sources.yaml`, severity classifications, `dewey doctor`/`dewey lint` surfacing. Ref: #131
- [x] 2.11 Add curated knowledge stores subsection: `dewey curate` command, `knowledge-stores.yaml` config, `curated` trust tier, background curation. Ref: #41
- [x] 2.12 Update learning identity examples to v3.2.0 timestamped format (`{tag}-{YYYYMMDDTHHMMSS}-{author}`). Add migration note and backward compatibility info. Ref: #132
- [x] 2.13 Add index pipeline performance note (batch embedding, concurrent source fetching). Ref: #207

## 3. Update Team Page (`content/docs/team/dewey.md`)

All tasks in this group modify the same file. No [P] markers — execute sequentially.

- [x] 3.1 Update tool count from 48 to 50 in the tool catalog header. Refs: #41, #113
- [x] 3.2 Add `curate` tool to the Knowledge Management category in the tool catalog. Ref: #41
- [x] 3.3 Add `store_compiled` tool to the Knowledge Management category in the tool catalog (alongside `compile`, `lint`, `promote`). Ref: #113
- [x] 3.4 Add `DEWEY_CHUNK_MAX_CHARS` env var reference alongside `max_chunk_chars` in the embedding model section. Ref: #208
- [x] 3.5 Add pluggable providers note to the embedding model section with link to getting-started guide. Ref: #113

## 4. Cross-Page Verification

- [x] 4.1 Verify tool count is consistently 50 across all three pages
- [x] 4.2 Verify installation methods are consistent between project page and knowledge page
- [x] 4.3 Run `npm run build` and confirm zero errors
- [x] 4.4 Run `npm run dev` and visually verify all three updated pages render correctly
- [x] 4.5 Verify constitution alignment: Observable Quality — all content traceable to issue numbers, no fabricated features
- [x] 4.6 Verify provider terminology is consistent across all three pages (Ollama and Vertex AI mentioned consistently)
- [x] 4.7 Verify `DEWEY_CHUNK_MAX_CHARS` documentation is consistent between knowledge page and team page (description, default value, config field mapping)
- [x] 4.8 Verify tool count against Dewey source (confirm exactly 50 tools before publishing)

<!-- spec-review: passed -->
<!-- code-review: passed -->
