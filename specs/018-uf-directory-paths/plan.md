# Implementation Plan: .uf/ Directory Path Update

**Branch**: `018-uf-directory-paths` | **Date**: 2026-04-06 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/018-uf-directory-paths/spec.md`

## Summary

Mechanical find-and-replace across content pages and internal agent/command files to update stale directory paths (`.dewey/`, `.hive/`, `.unbound-force/`, `.muti-mind/`, `.opencode/unbound/`) to their `.uf/`-consolidated equivalents per Spec 025. Also updates `.gitignore` entry.

## Technical Context

**Language/Version**: Markdown (Hugo content files)
**Primary Dependencies**: Hugo (via @thulite), Doks theme
**Storage**: N/A (static Markdown files)
**Testing**: `npm run build` (zero errors) + grep verification (zero stale paths)
**Target Platform**: GitHub Pages (static site)
**Project Type**: Static documentation site
**Performance Goals**: N/A
**Constraints**: No changes to files under `specs/` (historical records)
**Scale/Scope**: ~18 files, ~40 replacements

## Constitution Check

_GATE: Must pass before implementation._

| Principle             | Verdict  | Rationale                                                                                                                                                                                      |
| --------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| I. Content Accuracy   | **PASS** | Fixing stale filesystem paths to match the current state of upstream repos (Spec 025 landed). Paths like `.dewey/sources.yaml` no longer exist — the correct path is `.uf/dewey/sources.yaml`. |
| II. Minimal Footprint | **PASS** | Pure text edits in existing files. No new files, templates, CSS, or dependencies added.                                                                                                        |
| III. Visitor Clarity  | **PASS** | Developers following documented paths will find the correct files on their filesystem. Stale paths cause confusion; updated paths match reality.                                               |

## File Change Matrix

| File                                               | Old Patterns                            | Replacement Count |
| -------------------------------------------------- | --------------------------------------- | ----------------- |
| `content/docs/getting-started/knowledge.md`        | `.dewey/`, `.muti-mind/`                | ~12               |
| `content/docs/projects/dewey.md`                   | `.dewey/`                               | ~2                |
| `content/docs/team/dewey.md`                       | `.dewey/`                               | ~3                |
| `content/blog/dewey-knowledge-retrieval.md`        | `.dewey/`                               | ~2                |
| `content/docs/getting-started/developer.md`        | `.dewey/`, `.unbound-force/`            | ~3                |
| `content/docs/getting-started/common-workflows.md` | `.unbound-force/`                       | ~2                |
| `content/docs/projects/replicator.md`              | `.hive/`                                | ~1                |
| `.opencode/agents/cobalt-crush-dev.md`             | `.opencode/unbound/`, `.unbound-force/` | ~5                |
| `.opencode/agents/divisor-adversary.md`            | `.opencode/unbound/`                    | ~3                |
| `.opencode/agents/divisor-architect.md`            | `.opencode/unbound/`                    | ~1                |
| `.opencode/agents/divisor-guard.md`                | `.opencode/unbound/`                    | ~1                |
| `.opencode/agents/divisor-sre.md`                  | `.opencode/unbound/`                    | ~1                |
| `.opencode/agents/divisor-testing.md`              | `.opencode/unbound/`                    | ~1                |
| `.opencode/agents/divisor-techwriter.md`           | `.opencode/unbound/`                    | ~1                |
| `.opencode/agents/divisor-pr.md`                   | `.opencode/unbound/`                    | ~1                |
| `.opencode/command/review-council.md`              | `.opencode/unbound/`                    | ~1                |
| `.opencode/command/uf-init.md`                     | `.opencode/unbound/`                    | ~1                |
| `.opencode/command/cobalt-crush.md`                | `.opencode/unbound/`, `.unbound-force/` | ~2                |
| `.gitignore`                                       | `.dewey/`                               | ~1                |

**Total**: ~18 files, ~42 replacements

## Project Structure

### Documentation (this feature)

```text
specs/018-uf-directory-paths/
├── spec.md              # Feature specification
├── plan.md              # This file
├── tasks.md             # Task checklist
└── checklists/          # Requirement quality checklists
```

### Source Code (repository root)

No new source files. All changes are in-place text edits to existing Markdown files.
