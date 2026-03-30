# Research: Project Documentation Update

**Branch**: `012-project-docs-update` | **Date**: 2026-03-30

## Research Scope

This research captures all source material needed to accurately update the Unbound Force website documentation across three areas: Gaze v1.4.7, Dewey v1.4.2, and swarm workflow features. The authoritative sources are the gap analysis documents and upstream repository files.

---

## Area 1: Gaze v1.4.7 Documentation Updates

**Source**: `gaze-website-docs-audit.md` (2026-03-29), cross-referenced against Gaze codebase at HEAD.

### 1.1 Critical Factual Errors (US1)

#### `gaze init` File Count

**Current website claim**: "This creates four files in `.opencode/` that wire up the `/gaze` command and a quality reporting agent."

**Actual behavior**: `gaze init` creates **8 files** across 3 subdirectories:

| Directory               | Files                                                               |
| ----------------------- | ------------------------------------------------------------------- |
| `.opencode/agents/`     | `gaze-reporter.md`, `gaze-test-generator.md`, `reviewer-testing.md` |
| `.opencode/command/`    | `gaze.md`, `gaze-fix.md`, `speckit.testreview.md`                   |
| `.opencode/references/` | `doc-scoring-model.md`, `example-report.md`                         |

**Pages affected**: `content/docs/projects/gaze.md` (Setup section)

#### `/classify-docs` Command References

**Current website claim**: "A companion command, `/classify-docs`, enhances classification by combining mechanical signals with project documentation analysis."

**Reality**: The `/classify-docs` command and `doc-classifier` agent were **removed** in Gaze spec 012 (consolidate-classify-docs). Document-enhanced classification was inlined into the `gaze-reporter` agent.

**Pages affected**: `content/docs/projects/gaze.md` (Three Modes section). Must search all pages for any other references.

#### Assertion Mapping Accuracy

**Current website claim**: "Assertion mapping accuracy is ~78.8%"

**Actual value**: **84.7%** (83/98 mapped assertions). The ratchet floor is 84.0%.

**Improvement history**:

- Spec 007 (assertion-mapping-depth): 73.8% → 78.8%
- Spec 034 (container-assertion-mapping): 78.8% → 85.1%
- PR #84 (TypeAssertExpr fix): 85.1% → 84.7% (new fixture added more assertion sites)

The 90% target mentioned on the website is still correct.

**Pages affected**: `content/docs/projects/gaze.md` (Current Limitations section)

#### Missing `aireport` Package

**Current website lists**: analysis, taxonomy, classify, config, loader, report, crap, quality, docscan, scaffold (10 packages)

**Missing**: `aireport` — the AI CLI adapter integration package added in spec 018 (ci-report). This is a significant package (~10 files) that handles the `gaze report` command's AI pipeline.

**Pages affected**: `content/docs/projects/gaze.md` (Architecture table)

### 1.2 Missing Major Features (US2)

#### `--ai=opencode` Adapter

**Source**: Gaze spec 019

OpenCode is a fully supported AI backend for `gaze report`. The website only shows `--ai=claude`. The tester.md CI example and gaze.md usage section should include OpenCode as an option.

**Usage**: `gaze report ./... --ai=opencode`

**Pages affected**: `content/docs/projects/gaze.md`, `content/docs/getting-started/tester.md`

#### `--coverprofile` Flag

**Source**: Gaze spec 020

Allows passing a pre-generated Go coverage profile to skip the internal `go test` run. Eliminates CI double-test-run. This is the recommended CI pattern.

**Recommended pattern**:

```bash
# 1. Run tests with coverage profile
go test -race -count=1 -coverprofile=coverage.out ./...

# 2. Pass the profile to gaze report (skips internal test run)
gaze report ./... --ai=opencode \
  --coverprofile=coverage.out \
  --max-crapload=50 \
  --min-contract-coverage=50
```

**Pages affected**: `content/docs/getting-started/tester.md` (CI Integration Pattern)

#### `/gaze fix` Command

**Source**: gaze-test-generator agent

Batch test remediation via AI-assisted test generation. New command scaffolded by `gaze init`. Not mentioned anywhere on the website.

**Workflow**: The `/gaze fix` command reads quality data (gaps, fix strategies) and generates Go test functions to close coverage gaps. It uses the `gaze-test-generator` agent.

**Pages affected**: `content/docs/projects/gaze.md` or `content/docs/getting-started/tester.md`

#### Fix Strategy Labels

**Source**: report-actionability spec

Each CRAPload function now carries a remediation label visible in text and JSON output:

| Label                | Meaning                                             |
| -------------------- | --------------------------------------------------- |
| `add_tests`          | Function has no tests — write tests                 |
| `add_assertions`     | Tests exist but don't assert on contractual effects |
| `decompose`          | Function is too complex — split it                  |
| `decompose_and_test` | Both too complex and undertested                    |

**Pages affected**: `content/docs/projects/gaze.md`

### 1.3 Minor Updates (US3)

#### Sample Output Version

**Current**: "analyzed with Gaze v1.2.3"
**Current version**: v1.4.7. The sample output numbers may no longer match what a user would see.

**Decision**: Update the version reference or add a note that output may vary by version. The sample data itself (gcal-organizer analysis) can remain as a representative example.

#### CI Threshold Value

**Current**: `--max-crapload=15`
**Reality**: Gaze's own CI uses `--max-crapload=38`. A threshold of 15 is unrealistically low for most real projects.

**Recommendation**: Use `--max-crapload=50` as the example starting point with a note that the value depends on the project.

#### Single Package Loading Limitation

**Current**: "Single package loading. The `analyze` command processes one package at a time."
**Reality**: `gaze crap ./...` and `gaze report ./...` process the entire module. Only `gaze analyze` and `gaze quality` are single-package.

**Fix**: Scope the limitation to `gaze analyze` and `gaze quality` only.

#### P3-P4 Detection Text

**Current**: "P3-P4 side effects not yet detected."
**Issue**: P2 detection IS implemented (file system, database, goroutine, panic, context cancellation). The statement should clearly state P2 is detected and only P3-P4 are undetected.

**Fix**: Reword to: "P3-P4 side effects (stdout/stderr, environment mutations, mutex operations, reflection, unsafe) are not yet detected. P0 through P2 are fully implemented."

---

## Area 2: Dewey v1.4.2 Documentation Updates

**Source**: `dewey-gaps-2026-03-30.md` (2026-03-30), cross-referenced against Dewey codebase at HEAD.

### 2.1 Source Configuration and Ignore Behavior (US4)

#### `.gitignore` Support (Spec 006)

**What changed**: Spec 006-unified-ignore (merged 2026-03-30) added automatic `.gitignore` respect across all filesystem walkers.

**Key behavior**:

- `.gitignore` at the source root is automatically respected — no configuration needed
- Hidden directories (`.git/`, `.obsidian/`, `.dewey/`) are always skipped
- Pattern syntax matches `.gitignore` (directory names, globs, negation, comments)

**Impact on existing docs**: The Local Disk section in `knowledge.md` currently says "indexes all Markdown files in your repository, including hidden directories" — this is no longer fully accurate. With `.gitignore` respect, directories like `node_modules/`, `vendor/`, etc. are automatically skipped.

#### New `sources.yaml` Fields

Two new fields for disk sources:

| Field       | Type       | Default | Description                                                 |
| ----------- | ---------- | ------- | ----------------------------------------------------------- |
| `ignore`    | `[]string` | `[]`    | Additional ignore patterns (union-merged with `.gitignore`) |
| `recursive` | `bool`     | `true`  | When `false`, restricts indexing to top-level files only    |

**Example YAML**:

```yaml
sources:
  - id: disk-local
    type: disk
    name: my-project
    config:
      path: "."
      ignore:
        - "*.generated.go"
        - "testdata/"
      recursive: true
```

**Pages affected**: `content/docs/getting-started/knowledge.md` (Configure Content Sources > Local Disk)

### 2.2 Missing CLI Commands and Troubleshooting (US5)

#### `dewey doctor`

**Added**: v1.1.0, enhanced in v1.3.0 and v1.4.2

**Diagnostic sections**:

1. **Environment** — vault path, dewey binary location
2. **Workspace** — `.dewey/` directory, config files, `sources.yaml`
3. **Database** — `graph.db` health, page/block/embedding counts
4. **Sources in Database** — per-source page counts
5. **Embedding Layer** — Ollama availability, model status
6. **MCP Server** — lock file, `opencode.json` configuration
7. **Summary box** — overall health with emoji markers (✓/⚠/✗)

**Usage**: `dewey doctor`

#### `dewey reindex`

**Added**: v1.2.0

**Behavior**: Deletes `graph.db` and re-indexes from scratch. Use when the index is corrupted or stale.

**Usage**: `dewey reindex`

#### Global CLI Flags

**Added**: v1.3.0+

| Flag              | Short | Description                                                            |
| ----------------- | ----- | ---------------------------------------------------------------------- |
| `--verbose`       | `-v`  | Enable debug logging                                                   |
| `--log-file PATH` |       | Write logs to file (auto-logging to `.dewey/dewey.log` also available) |
| `--no-embeddings` |       | Skip embedding generation (useful for quick indexing without Ollama)   |
| `--vault PATH`    |       | Specify vault directory (default: current directory)                   |

#### Troubleshooting Section Content

Common issues and resolution steps to document:

| Issue                  | Symptoms                               | Resolution                                                                                                                                  |
| ---------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| MCP server timeout     | OpenCode shows connection timeout      | Check `.gitignore` for large directories being indexed; run `dewey reindex`                                                                 |
| Ollama not running     | `dewey doctor` shows embedding layer ✗ | Run `ollama serve` or install Ollama (`brew install --cask ollama`)                                                                         |
| Lock file conflicts    | "Another dewey instance is running"    | Only one `dewey serve` per vault; check for stale `.dewey/dewey.lock`                                                                       |
| Low embedding coverage | Semantic search returns few results    | Run `dewey index` to generate embeddings for new content                                                                                    |
| Slow startup           | First `dewey serve` takes minutes      | Normal for large repos on first index; subsequent startups are near-instant. Check `.gitignore` to exclude `node_modules/`, `vendor/`, etc. |

**Pages affected**: `content/docs/getting-started/knowledge.md` (new sections)

### 2.3 Dewey Release History (Context)

| Version | Date       | Key Changes                                                    |
| ------- | ---------- | -------------------------------------------------------------- |
| v1.0.0  | 2026-03-22 | Unified content serve, external source loading                 |
| v1.1.0  | 2026-03-23 | `dewey doctor`, `--verbose`, `--no-embeddings`                 |
| v1.2.0  | 2026-03-23 | `dewey reindex`, hard error on persistence failures            |
| v1.3.0  | 2026-03-24 | Auto-logging, `--log-file`, enhanced doctor diagnostics        |
| v1.4.0  | 2026-03-24 | `resolveVaultPath()`, `--vault` flag                           |
| v1.4.1  | 2026-03-25 | Embedding chunk size 2000→1000                                 |
| v1.4.2  | 2026-03-30 | Doctor emoji markers, lipgloss bold headings                   |
| _main_  | 2026-03-30 | Spec 006: unified `.gitignore` + `sources.yaml` ignore support |

---

## Area 3: Swarm Workflow Features

### 3.1 `/unleash` Command (US6)

**Source**: `/Users/jflowers/Projects/github/unbound-force/unbound-force/.opencode/command/unleash.md`

#### Overview

`/unleash` is the autonomous Speckit pipeline execution command. It takes a spec from draft to demo-ready code in a single command. Orchestrates 8 steps with graceful exit points and full resumability.

#### The 8-Step Pipeline

| Step | Name              | Description                                                                                                                                                                                                |
| ---- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | **Clarify**       | Scans spec.md for `[NEEDS CLARIFICATION]` markers. Uses Dewey semantic search to auto-resolve questions. Exits to human only for unanswerable questions.                                                   |
| 2    | **Plan**          | Delegates to Cobalt-Crush to generate `plan.md` via `/speckit.plan`                                                                                                                                        |
| 3    | **Tasks**         | Delegates to Cobalt-Crush to generate `tasks.md` via `/speckit.tasks`                                                                                                                                      |
| 4    | **Spec Review**   | Runs the review council in Spec Review Mode. Subsumes `/speckit.analyze` and `/speckit.checklist`. Auto-fixes LOW/MEDIUM findings. Exits on HIGH/CRITICAL.                                                 |
| 5    | **Implement**     | Parses `tasks.md` for phases. Sequential tasks run first, then `[P]` parallel tasks via Swarm worktrees (up to 4 concurrent workers). Phase checkpoints run CI commands derived from `.github/workflows/`. |
| 6    | **Code Review**   | Runs the review council in Code Review Mode. Includes Phase 1a CI hard gate, Phase 1b Gaze quality analysis (if available), and Divisor agent reviews. Up to 3 fix iterations.                             |
| 7    | **Retrospective** | Analyzes the session and stores learnings in Hivemind semantic memory. Categories: patterns, gotchas, review insights, file-specific learnings.                                                            |
| 8    | **Demo**          | Presents structured demo instructions: what was built, how to verify, key files changed, test results, and next steps (including `/finale`).                                                               |

#### Key Capabilities

- **Dewey-powered clarification**: Auto-resolves spec ambiguities using semantic search. Falls back to human input when Dewey is unavailable or results are insufficient.
- **Parallel Swarm workers**: `[P]`-marked tasks execute in parallel via git worktrees (up to 4 concurrent). Falls back to sequential when Swarm plugin is not installed.
- **Resumability**: Probes filesystem state on startup to detect completed steps. Resumes from the first incomplete step. All progress is persisted in spec artifacts (plan.md, tasks.md checkboxes, spec-review marker).
- **Graceful exit points**: Every exit (unanswerable questions, HIGH/CRITICAL findings, worker failures, merge conflicts, test failures, review exhaustion) includes actionable next steps and resume instructions.
- **CI command derivation**: Build and test commands are derived from `.github/workflows/` files, not hardcoded. Same pattern used by `/review-council` Phase 1a.

#### Branch Safety

- NEVER runs on `main` — requires a Speckit feature branch (`NNN-*`)
- Rejects `opsx/*` branches (use `/opsx:apply` instead)
- Validates spec.md exists before proceeding

#### Relationship to `/finale`

`/unleash` builds; `/finale` ships. After `/unleash` completes, the demo step suggests running `/finale` to commit, push, create PR, merge, and return to main.

### 3.2 `/finale` Command (US6)

**Source**: `/Users/jflowers/Projects/github/unbound-force/unbound-force/.opencode/command/finale.md`

#### Overview

`/finale` automates the end-of-branch workflow. One command to wrap up any feature or OpenSpec branch: stage, commit, push, create PR, watch CI, merge, and return to main.

#### The 9-Step Workflow

| Step | Name                        | Description                                                                         |
| ---- | --------------------------- | ----------------------------------------------------------------------------------- |
| 1    | **Branch Safety Gate**      | Verifies not on `main`. Notes the branch name.                                      |
| 2    | **Check for Changes**       | Inspects working tree. If clean, checks for unpushed commits or existing PR.        |
| 3    | **Generate Commit Message** | Analyzes staged changes, generates conventional commit message, shows for approval. |
| 4    | **Push to Remote**          | Sets upstream if needed (`git push -u origin <branch>`).                            |
| 5    | **Create or Find PR**       | Creates PR via `gh pr create` or finds existing one.                                |
| 6    | **Watch CI Checks**         | `gh pr checks --watch`. Stops on failure with options.                              |
| 7    | **Merge PR**                | `gh pr merge --rebase --delete-branch`. Rebase merge strategy.                      |
| 8    | **Return to Main**          | `git checkout main && git pull`.                                                    |
| 9    | **Summary**                 | Displays completion report: branch, commit, PR, checks, status.                     |

#### Key Capabilities

- **Works with both branch types**: Speckit (`NNN-*`) and OpenSpec (`opsx/*`) branches
- **Secrets check**: Scans for `.env`, `credentials.json`, `*.key`, `*.pem` before staging
- **Commit message approval**: Shows proposed message, allows editing
- **Rebase merge strategy**: Always uses `--rebase` (no squash or merge commits)
- **CI watching**: Waits for checks to pass before merging. Never auto-merges with failing checks.
- **Branch cleanup**: Deletes branch via `--delete-branch` on merge

#### Guardrails

- NEVER runs on `main`
- NEVER merges with failing checks
- NEVER stages secret files without warning
- NEVER commits without user approval of the message
- ALWAYS uses rebase merge
- If any step fails, stops immediately with context and options

### 3.3 Review Council CI Gate and Gaze Integration (US7)

**Source**: `/review-council` command documentation, Spec 019 (Divisor Council Refinement)

#### Phase 1a: CI Hard Gate

Before delegating to Divisor agents, the review council runs a CI hard gate:

1. **Derives commands from `.github/workflows/`** — reads CI workflow files to identify the exact build, test, vet, lint, and vulnerability check commands
2. **Executes locally** — runs the derived commands (e.g., `go build`, `go test`, `go vet`, `golangci-lint run`, `govulncheck ./...`)
3. **Gate behavior** — any non-zero exit code is a gate failure. The review stops before invoking Divisor agents.

This ensures the code compiles, tests pass, and static analysis is clean before spending review tokens on Divisor agents.

#### Phase 1b: Conditional Gaze Quality Analysis

If Gaze is installed, the review council runs quality analysis and passes the results as context to Divisor agents:

1. **Runs `gaze report`** — generates CRAP scores, contract coverage, and quality findings
2. **Passes to reviewers** — the Testing persona uses Gaze data as evidence for coverage assessment
3. **Non-blocking** — if Gaze is not installed, Phase 1b is skipped with an informational note

### 3.4 Divisor Council Refinements (US7)

**Source**: Spec 019 (Divisor Council Refinement) — `unbound-force/unbound-force/specs/019-divisor-council-refinement/`

#### Exclusive Ownership Boundaries

Each review dimension is owned by exactly one Divisor persona, eliminating duplicate findings:

| Dimension                                   | Owner     |
| ------------------------------------------- | --------- |
| Secrets/credentials                         | Adversary |
| Dependency CVEs/supply chain                | Adversary |
| Error handling/resilience                   | Adversary |
| Test isolation/coverage/depth               | Tester    |
| Plan alignment/intent drift                 | Guard     |
| Zero-waste mandate                          | Guard     |
| Constitution alignment                      | Guard     |
| File permissions/hardcoded config           | SRE       |
| Efficiency/performance (O(n²), allocations) | SRE       |
| Architectural patterns/conventions          | Architect |

#### Shared Severity Standard (`severity.md`)

A new convention pack file at `.opencode/unbound/packs/severity.md` defines shared severity levels used by all 5 Divisor personas:

| Level        | Definition                                                                           | Auto-Fix?                    |
| ------------ | ------------------------------------------------------------------------------------ | ---------------------------- |
| **CRITICAL** | Data loss, security breach, build failure, constitutional violation. MUST NOT merge. | Report only                  |
| **HIGH**     | Significant risk or tech debt. Blocks review.                                        | Report only                  |
| **MEDIUM**   | Quality issue, should be addressed. Non-blocking.                                    | Auto-fix in Spec Review Mode |
| **LOW**      | Minor style or documentation improvement. Non-blocking.                              | Auto-fix in Spec Review Mode |

Each level includes domain-specific examples per persona (e.g., Adversary CRITICAL: "Hardcoded production secret, SQL injection vector, panic in library code").

#### Prior Learnings Integration

All 5 Divisor agents include a "Prior Learnings" step at the start of their review workflow:

1. Search Hivemind for learnings tagged with the repo name and relevant file paths
2. Include found learnings as prior knowledge in the review context
3. Graceful degradation when Hivemind is not available (skip with informational note)

### 3.5 Convention Pack Count Update (US7)

**Current website claim**: 6 convention pack files (in `developer.md` Convention Packs table)

**Actual count**: **7 files** — the 6 existing files plus `severity.md`:

| File                   | Ownership  | Purpose                                                       |
| ---------------------- | ---------- | ------------------------------------------------------------- |
| `default.md`           | Tool-owned | Language-agnostic rules                                       |
| `default-custom.md`    | User-owned | Project-specific conventions                                  |
| `go.md`                | Tool-owned | Go-specific rules                                             |
| `go-custom.md`         | User-owned | Project-specific Go conventions                               |
| `typescript.md`        | Tool-owned | TypeScript-specific rules                                     |
| `typescript-custom.md` | User-owned | Project-specific TypeScript conventions                       |
| `severity.md`          | Tool-owned | **NEW**: Shared severity definitions for all Divisor personas |

The `severity.md` pack is language-agnostic (always deployed, like `default.md`) and tool-owned (auto-updated by `uf init`). It is NOT customizable per-repo — severity definitions should be consistent across all projects.

### 3.6 `uf init` and `uf setup` Updates (US8)

**Source**: `internal/scaffold/scaffold.go` from the unbound-force meta repo

#### `opencode.json` Management Moved to `uf init`

Per Spec 017, `opencode.json` management moved from `uf setup` to `uf init`. The `configureOpencodeJSON()` function in `scaffold.go`:

1. **Dewey MCP entry**: When `dewey` is in PATH, adds/updates the `mcp.dewey` entry:

   ```json
   {
     "mcp": {
       "dewey": {
         "type": "local",
         "command": ["dewey", "serve", "--vault", "."],
         "enabled": true
       }
     }
   }
   ```

2. **Swarm plugin entry**: When `.hive/` directory exists, adds `"opencode-swarm-plugin"` to the `plugin` array.

3. **Idempotent**: Checks for existing entries before adding. Uses `--force` to overwrite stale entries.

4. **Preserves user keys**: Uses `map[string]json.RawMessage` to preserve unknown user keys (custom MCP servers, custom config). Per SOLID Open/Closed Principle.

#### `uf init` Sub-Tool Initialization

After deploying files, `uf init` performs:

1. Creates `.unbound-force/config.yaml` (skipped if exists)
2. If Dewey is available: `dewey init` + auto-detect sibling repos for sources + `dewey index`
3. If `--force` and Dewey workspace exists: `dewey index` (re-index)
4. Configures `opencode.json` with Dewey MCP and Swarm plugin entries

#### `uf setup` Step Count

The `opencode.json` management move from `uf setup` to `uf init` reduced setup steps. The current `uf setup` step count should be verified against the actual implementation, but the documentation should reflect that `opencode.json` is now managed by `uf init`, not `uf setup`.

#### Legacy File Detection

`uf init` now detects previously scaffolded `reviewer-*.md` files and prints a warning:

```
⚠  Legacy reviewer agents detected:
    reviewer-adversary.md
    reviewer-architect.md
    ...
  These have been superseded by divisor-* agents.
  Remove with: rm .opencode/agents/reviewer-*.md
```

It does NOT delete the files — warn only.

---

## Cross-Cutting Concerns

### Page Inventory

Pages that need updates based on this research:

| Page                      | File                                               | User Stories  |
| ------------------------- | -------------------------------------------------- | ------------- |
| Gaze project page         | `content/docs/projects/gaze.md`                    | US1, US2, US3 |
| Tester getting-started    | `content/docs/getting-started/tester.md`           | US2, US3      |
| Knowledge getting-started | `content/docs/getting-started/knowledge.md`        | US4, US5      |
| Common Workflows          | `content/docs/getting-started/common-workflows.md` | US6, US7, US8 |
| Developer getting-started | `content/docs/getting-started/developer.md`        | US7, US8      |
| The Divisor team page     | `content/docs/team/the-divisor.md`                 | US7           |

### Cross-Page Consistency Checks

After all updates, verify these values are consistent across all pages:

| Claim                      | Correct Value                  | Pages to Check     |
| -------------------------- | ------------------------------ | ------------------ |
| Convention pack count      | 7 files                        | developer.md       |
| Assertion mapping accuracy | ~84.7%                         | gaze.md            |
| `gaze init` file count     | 8 files                        | gaze.md            |
| AI adapter options         | `--ai=opencode`, `--ai=claude` | gaze.md, tester.md |
| `--max-crapload` example   | 50 (realistic)                 | tester.md          |
| P2 detection status        | Implemented                    | gaze.md, tester.md |

### Version Context Strategy

For features that require a specific minimum version, add version context where appropriate (e.g., "added in v1.3.0"). This helps users with older installations understand feature availability without cluttering every mention.

### Edge Case Documentation

Per spec edge cases:

- Gaze version-specific features: Add version context for features not in older versions
- Dewey troubleshooting without Ollama: Cover the Ollama-not-running scenario explicitly
- `/unleash` without Dewey: Document graceful degradation (falls back to human-driven clarification)
- Convention pack count changes: Focus on describing the pack structure rather than hardcoding a count that may change
