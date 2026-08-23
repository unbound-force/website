# Tasks: Gaze Multi-Language Documentation Update

## Group 1: Update Gaze Project Page (`content/docs/projects/gaze.md`) — Sequential

- [x] 1.1 Update frontmatter: revise `description` and `lead` to replace "for Go" with multi-language framing (Issue #227)
- [x] 1.2 Add External Analyzers section after CLI Flags: document JSON-RPC 2.0 protocol, three-tier discovery (CLI flag → `.gaze.yaml` → PATH convention), `--language` flag, link to `docs/protocol.md`, mention streaming mode (Issue #227)
- [x] 1.3 Update CLI flags table: add `--analyzer <binary>` (crap/quality/report), `--language <lang>` (crap/quality/report), `--test-short` (crap/report/self-check) (Issues #227, #230)
- [x] 1.4 Expand side effect taxonomy: add Universal Side Effect Types subsection listing 10 new types by tier — `ErrorSignal` (P0); `GeneratorYield`, `ContainerMutation`, `StreamOutput` (P1); `AsyncGeneratorYield`, `MetaprogrammingMutation`, `DescriptorEffect`, `ResourceManagement`, `ImportSideEffect`, `MonkeyPatch` (P2). Document `Detail` metadata field. Link to protocol docs for complete reference. Do not reproduce the full 48-type reference table — link to protocol docs instead (Issue #229)
- [x] 1.5 Update architecture package table: verify against upstream codebase and add any new packages (e.g., `protocol`, `provider`, `adapter`, `cliutil`) (Issue #227)
- [x] 1.6 Add Migration Notes section before "Learn More": (a) JSON Output Changes — `go_version` → `language_version`, new `language` field (Issue #228); (b) 7 language-neutral `SideEffectType` aliases (Issue #228); (c) Coverage Behavior Change — full test suite default, `--test-short` opt-in, `GAZE_COVERAGE_RUN=1`, CRAP score impact (Issue #230); (d) CRAP score changes users may observe after upgrade (Issue #230)
- [x] 1.7 Review and update the "Current Limitations" section for accuracy against the expanded taxonomy — scope P3-P4 limitation to "(Go analysis)", note external analyzers define their own detection scope

## Group 2: Update Homepage (`layouts/home.html`) — Parallel

- [x] 2.1 [P] Update Gaze card badge from "Go" to "Go + Multi-Language" (Issue #227)

## Group 3: Update Tester Guide and Related Pages — Parallel

- [x] 3.1 [P] Update tester guide (`content/docs/getting-started/tester.md`): add `--test-short` migration callout in CI section, add `--test-short` example in CI code block, update side effects count from "30+ types" to reflect the expanded taxonomy for consistency with the project page (Issues #228, #230)
- [x] 3.2 [P] Update `content/docs/team/gaze-tester.md`: revise framing from "for Go" to multi-language, add reference to external analyzer protocol (Issue #227)
- [x] 3.3 [P] Update `content/docs/projects/_index.md`: revise Gaze description from "for Go" to multi-language framing (Issue #227)

## Group 4: Verification

- [x] 4.1 Run `npm run build` — must succeed with no new warnings
- [x] 4.2 Visual verification: (a) all new sections render on Gaze project page, (b) homepage card shows updated badge, (c) tester guide shows `--test-short` docs, (d) team page and projects index show updated framing, (e) all internal links resolve, (f) dark mode renders correctly on the three modified pages (Gaze project page, homepage, tester guide)
- [x] 4.3 Constitution alignment verification — verify that all CLI flag names, JSON field names, and side effect type names documented on the updated pages match the upstream Gaze codebase. Cross-reference against PR descriptions for gaze#178, gaze#180, gaze#184, gaze#194
- [x] 4.4 Upstream PR merge verification gate — verify all four upstream PRs (gaze#178, gaze#180, gaze#184, gaze#194) are merged before proceeding to merge the website PR. If any PR is not merged, defer the corresponding documentation sections

<!-- spec-review: passed -->
<!-- code-review: passed -->
