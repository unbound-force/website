# Research: v0.10.0 / v0.11.0 Documentation Sync

**Branch**: `023-docs-v10-v11-sync` | **Date**: 2026-04-11

## Research Objective

Understand the upstream changes from PRs #85 (gatekeeping), #87 (gitignore-init), and #89 (Curator) to determine exactly what content needs to be added or updated across 6 existing website pages. Identify all occurrences of stale persona counts and map each functional requirement to concrete content changes.

## Source 1: PR #89 — The Curator Agent (Issue #35)

**Source**: `unbound-force/unbound-force` PR #89 (closed/merged)
**Dewey index**: `github-unbound-force/unbound-force/pulls/89`

### Key Facts

- The Curator is the **9th Divisor persona** — the first triage-layer persona, sitting between the 5 review personas and 3 content creation personas.
- **Role**: Documentation & Content Pipeline Triage — detects documentation gaps during code review and auto-files GitHub issues in `unbound-force/website`.
- **Functional layers**: Review (5: Guard, Architect, Adversary, SRE, Tester) → Triage (1: Curator) → Creation (3: Scribe, Herald, Envoy).
- **Bash access**: Restricted to exactly two operations — `gh issue list` and `gh issue create` (both scoped to `unbound-force/website`). All other bash usage is a contract violation.
- **Temperature**: 0.2 (precision — same as The Scribe).
- **Scope boundary**: Triage, not creation. The Curator identifies _what_ needs documenting and files tracking issues. It does NOT write documentation, blog posts, or tutorials.

### Five Audit Checklist Items

1. **Documentation gap detection** — Were AGENTS.md or README files updated when the PR changes user-facing behavior?
2. **Website documentation issue check** — Should an issue be filed in the website repository?
3. **Duplicate issue check** — Search existing open issues before filing new ones.
4. **Blog opportunity identification** — Does this change warrant a blog post?
5. **Tutorial opportunity identification** — Does this change introduce a workflow engineers need to learn?

### Exclusive Ownership Domain

- **Documentation & Content Pipeline Triage**: documentation gap detection, blog opportunity identification, tutorial opportunity identification, and cross-repo issue filing in `unbound-force/website`.

### Blog Post Cross-Reference

The Curator blog post (`content/blog/curator-blog.md`, spec 022) references:

- "nine personas" (line 47)
- "9th Divisor persona" (line 29)
- "all nine agents" (line 105)
- Links to the Divisor team page (`/docs/team/the-divisor/`) which currently says "eight"

**This is the highest-priority contradiction to resolve.**

## Source 2: PR #85 — Gatekeeping Value Protection (Issue #33)

**Source**: `unbound-force/unbound-force` PR #85 (closed/merged)
**Dewey index**: `github-unbound-force/unbound-force/pulls/85`

### Key Facts

- Adds behavioral rules preventing AI agents from modifying gatekeeping values to make implementations pass.
- Core principle: When an agent cannot meet a quality gate, it **stops and reports the conflict** rather than weakening the gate.
- Modifying a gate without human authorization is a **CRITICAL-severity constitution violation**.

### Eight Protected Value Categories

1. **Coverage thresholds and CRAP scores** — minimum coverage percentages, CRAP score limits, coverage ratchets
2. **Severity definitions and auto-fix policies** — CRITICAL/HIGH/MEDIUM/LOW boundaries, auto-fix eligibility rules
3. **Convention pack rule classifications** — MUST/SHOULD/MAY designations (downgrading MUST to SHOULD is prohibited)
4. **CI flags and linter configuration** — `-race`, `-count=1`, `govulncheck`, `golangci-lint` rules, pinned action SHAs
5. **Agent temperature and tool-access settings** — frontmatter `temperature`, `tools.write`, `tools.edit`, `tools.bash` restrictions
6. **Constitution MUST rules** — any MUST rule in `.specify/memory/constitution.md` or hero definitions
7. **Review iteration limits and worker concurrency** — max review cycles, max parallel workers
8. **Workflow gate markers** — spec commit gates, phase checkpoint markers, review approval requirements

### Divisor Agent Changes

- **Guard** (`divisor-guard.md`): New "Gatekeeping Integrity" checklist item in both Code Review and Spec Review audit sections — detects unauthorized gate modifications.
- **Adversary** (`divisor-adversary.md`): New "Gate Tampering" checklist item in Security Checks section — detects removal or weakening of CI security controls.

## Source 3: PR #87 — ensureGitignore (Issue #34)

**Source**: `unbound-force/unbound-force` PR #87 (closed/merged)
**Dewey index**: `github-unbound-force/unbound-force/pulls/87`

### Key Facts

- New `ensureGitignore()` function in the scaffold engine, called during `uf init`.
- **Behavior**: Appends a standard ignore block to `.gitignore`.
- **Idempotent**: Uses marker comment `# Unbound Force — managed by uf init` — safe to run multiple times.
- **Creates `.gitignore`** if it does not exist (different from append-only for existing files).
- **Append-only**: Never overwrites or removes existing `.gitignore` content.
- Called from `Run()` after config generation, before sub-tool delegation.

### Ignore Block Content

The block covers two categories:

1. **`.uf/` runtime data**:
   - `.uf/workflows/` and `.uf/artifacts/`
   - `.uf/dewey/` runtime (graph.db, cache/, locks, log)
   - `.uf/replicator/` runtime (databases, locks)
   - `.uf/muti-mind/artifacts/`
   - `.uf/mx-f/data/`

2. **Legacy directories**:
   - `.dewey/`
   - `.hive/`
   - `.unbound-force/`
   - `.muti-mind/`
   - `.mx-f/`

## Current State Audit: "Eight" Occurrences

Grep results for "eight" in the context of Divisor/personas across `content/`:

### the-divisor.md (7 word-level occurrences across 6 lines — all must change to "nine")

| Line | Current Text                                                                 | Context                     |
| ---- | ---------------------------------------------------------------------------- | --------------------------- |
| 3    | `description: "...eight sub-personas..."`                                    | Frontmatter description     |
| 17   | `council of eight dynamically discovered personas` + `eight canonical roles` | Council structure paragraph |
| 126  | `All eight personas share a calibration standard`                            | Severity standard section   |
| 139  | `All eight Divisor personas include a "Prior Learnings" step`                | Prior learnings section     |
| 151  | `passes all eight lenses`                                                    | Collective approval section |
| 155  | `multi-faceted feedback from the eight personas`                             | How Divisor serves the team |

### team/\_index.md (1 occurrence)

| Line | Current Text                    |
| ---- | ------------------------------- |
| 31   | `council of eight sub-personas` |

### contributing/\_index.md (1 occurrence)

| Line | Current Text                    |
| ---- | ------------------------------- |
| 33   | `council of eight sub-personas` |

### common-workflows.md (0 direct "eight" occurrences)

The code review table on lines 366-372 lists 5 personas (Guard, Architect, Adversary, Testing, SRE). The Curator needs to be added as a 6th row. No text says "five review personas" explicitly on this page — the table speaks for itself.

### Blog posts (NOT modified — reference only)

- `curator-blog.md` line 55: "All eight previous Divisor personas" — historical context, correct as-is
- `curator-blog.md` line 73: "reviewed by the existing eight personas" — historical context, correct as-is

## Affected Pages Summary

| Page                                               | Issue    | Changes Required                                                                                                                                                                                    |
| -------------------------------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `content/docs/team/the-divisor.md`                 | #33, #35 | Update 6× "eight"→"nine", add Curator section, add Curator to ownership table, add Guard "Gatekeeping Integrity" + Adversary "Gate Tampering" audit items, update lens count in collective approval |
| `content/docs/team/_index.md`                      | #35      | Update 1× "eight"→"nine" in Divisor summary                                                                                                                                                         |
| `content/docs/contributing/_index.md`              | #35      | Update 1× "eight"→"nine" in Divisor description                                                                                                                                                     |
| `content/docs/getting-started/common-workflows.md` | #35      | Add Curator row to code review persona table                                                                                                                                                        |
| `content/docs/getting-started/developer.md`        | #33, #34 | Add gatekeeping section, add `.gitignore` to `uf init` docs                                                                                                                                         |
| `content/docs/getting-started/quick-start.md`      | #34      | Mention `.gitignore` management in setup description                                                                                                                                                |

## Research Gaps

None identified. All three upstream PRs are merged and indexed in Dewey. The Curator agent definition file (`.opencode/agents/divisor-curator.md`) is available locally with full audit checklist details. All affected pages have been read and audited for stale content.
