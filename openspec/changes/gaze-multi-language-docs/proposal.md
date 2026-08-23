# Proposal: Gaze Multi-Language Documentation Update

## Why

Gaze has evolved from a Go-only test quality analysis tool to a multi-language analysis framework through four upstream PRs:

- **gaze#178** — External analyzer protocol (JSON-RPC 2.0) with `--analyzer` and `--language` CLI flags, `.gaze.yaml` analyzer configuration, three-tier discovery mechanism
- **gaze#180** — Breaking JSON change: `go_version` renamed to `language_version`, new `language` field, 7 language-neutral `SideEffectType` aliases, streaming protocol mode
- **gaze#184** — Expanded side effect taxonomy: 10 new universal `SideEffectType` constants across P0-P2 tiers, `SideEffect.Detail` metadata field, protocol v1.1.0
- **gaze#194** — `--test-short` CLI flag, coverage behavior change (full test suite by default), `GAZE_COVERAGE_RUN=1` env var

The website documentation currently describes Gaze as a Go-only tool and does not reflect these changes.

## What

Update the Unbound Force website to accurately document Gaze's multi-language support, new CLI flags, expanded side effect taxonomy, and breaking changes requiring migration guidance.

### Capabilities

1. **External Analyzer Protocol Documentation** — New section on the Gaze project page explaining JSON-RPC 2.0 protocol, three-tier discovery, and how to use external analyzers
2. **Migration Notes** — In-page section documenting breaking JSON changes (`go_version` → `language_version`), coverage behavior change, and CRAP score impact
3. **Expanded Taxonomy Documentation** — Updated side effect taxonomy with 10 new universal types listed by tier
4. **CLI Flag Updates** — Three new flags (`--analyzer`, `--language`, `--test-short`) added to the flags table
5. **Cross-Page Consistency** — Homepage card, tester guide, team page, and projects index updated to reflect multi-language framing

### Impact

- **`content/docs/projects/gaze.md`** — Primary update target: frontmatter, new sections, updated tables
- **`layouts/home.html`** — Homepage Gaze card badge update
- **`content/docs/getting-started/tester.md`** — `--test-short` documentation, side effect count update
- **`content/_index.md`** — Reviewed; lead text does not reference Go-only analysis. No changes needed
- **`content/docs/team/gaze-tester.md`** — Update framing from "for Go" to multi-language
- **`content/docs/projects/_index.md`** — Update Gaze description from "for Go" to multi-language

### Tracking

- GitHub Issue #227 — External analyzer protocol and new CLI flags
- GitHub Issue #228 — Breaking JSON change: `go_version` → `language_version`
- GitHub Issue #229 — Expanded side effect taxonomy: 10 new universal types
- GitHub Issue #230 — `--test-short` flag and coverage behavior change

## Constitution Alignment

### I. Autonomous Collaboration

**Assessment**: N/A — This is a documentation-only change to a static website. No agent collaboration patterns are affected.

### II. Composability First

**Assessment**: N/A — No agent, command, or workflow changes are introduced.

### III. Observable Quality

**Assessment**: PASS — This change directly supports Observable Quality by documenting Gaze's machine-parseable output changes (JSON field renames, new fields, expanded taxonomy). Users who consume Gaze's JSON output need accurate documentation to build integrations and CI pipelines. The migration notes ensure existing consumers can adapt to breaking changes.

### IV. Testability

**Assessment**: PASS — This is a documentation-only change to a static website with no automated test suite. The coverage strategy is manual validation: `npm run build` succeeds (task 4.1), visual verification of all affected pages (task 4.2), content accuracy verification against upstream sources (task 4.3), and upstream PR merge verification (task 4.4). This aligns with the project's validation model defined in AGENTS.md.

### V. Security by Default

**Assessment**: N/A — Documentation changes do not introduce dependencies, external inputs, or security-sensitive operations. No CI pipeline changes, no new npm packages, no configuration modifications.
