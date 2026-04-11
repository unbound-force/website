# Implementation Plan: v0.10.0 / v0.11.0 Documentation Sync

**Branch**: `023-docs-v10-v11-sync` | **Date**: 2026-04-11 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/023-docs-v10-v11-sync/spec.md`

## Summary

Synchronize website documentation with three upstream changes from `unbound-force/unbound-force`: The Curator agent (PR #89), gatekeeping value protection rules (PR #85), and `.gitignore` management via `uf init` (PR #87). This is a content-only update to 6 existing Markdown pages — no new pages, no layout changes, no code. The highest-priority fix resolves a persona count contradiction between the published Curator blog post ("nine personas") and the Divisor team page ("eight personas").

## Technical Context

**Language/Version**: Markdown (Hugo content files) + Hugo (via @thulite), Doks theme (`@thulite/doks-core`)
**Primary Dependencies**: N/A (static Markdown file edits only)
**Storage**: N/A (static site, no database)
**Testing**: Manual — `npm run build` must succeed; visual verification of 6 pages in light and dark mode
**Target Platform**: GitHub Pages (static site)
**Project Type**: Documentation website (Hugo SSG)
**Performance Goals**: N/A (no runtime performance impact — static content)
**Constraints**: All content must be sourced from upstream PRs #85, #87, #89. No fabrication.
**Scale/Scope**: 6 existing pages, ~85 lines of Markdown changes

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### Pre-Research Check

| Principle                 | Verdict | Rationale                                                                                                                                                                                                                                                                                                   |
| ------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **I. Content Accuracy**   | ✅ PASS | All content is sourced from merged upstream PRs (#85, #87, #89) indexed in Dewey. The Curator agent definition file (`.opencode/agents/divisor-curator.md`) is available locally. No capabilities are fabricated. The persona count update from "eight" to "nine" corrects an existing inaccuracy.          |
| **II. Minimal Footprint** | ✅ PASS | All changes are Markdown edits to existing pages. No new pages, no custom HTML, no SCSS, no JavaScript, no new dependencies. This is the simplest possible implementation.                                                                                                                                  |
| **III. Visitor Clarity**  | ✅ PASS | The primary fix (persona count consistency) directly improves visitor clarity — a reader following the blog post link to the team page will no longer encounter contradictory information. Gatekeeping and `.gitignore` documentation help developers understand agent behavior and `uf init` side effects. |

### Post-Design Re-Check

| Principle                 | Verdict | Rationale                                                                                                                                                                                                                                                                                                                                                         |
| ------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **I. Content Accuracy**   | ✅ PASS | Research phase confirmed all source material via Dewey semantic search. Eight protected value categories verified against upstream `AGENTS.md`. Curator audit checklist verified against local agent definition file. `.gitignore` behavior verified against upstream OpenSpec design doc.                                                                        |
| **II. Minimal Footprint** | ✅ PASS | No design artifacts (data-model, contracts) are needed — this is pure content. The quickstart.md verification steps use only existing tooling (`npm run build`, `npm run dev`, `grep`).                                                                                                                                                                           |
| **III. Visitor Clarity**  | ✅ PASS | Each content addition serves a clear visitor purpose: (1) Curator documentation lets blog readers find consistent information, (2) gatekeeping documentation explains why agents stop mid-implementation, (3) `.gitignore` documentation explains expected `uf init` side effects. No content is added without a clear "why should a visitor care" justification. |

No violations. No complexity tracking needed.

## Project Structure

### Documentation (this feature)

```text
specs/023-docs-v10-v11-sync/
├── spec.md              # Feature specification (3 consolidated GitHub issues)
├── plan.md              # This file
├── research.md          # Phase 0 output — upstream PR research via Dewey
└── quickstart.md        # Phase 1 output — verification steps and file change map
```

No data-model.md or contracts/ needed — this is a content-only change with no data structures, APIs, or interfaces.

### Source Code (repository root)

```text
content/
├── docs/
│   ├── team/
│   │   ├── the-divisor.md       # FR-001 through FR-003, FR-008 (Curator + gatekeeping audit items)
│   │   └── _index.md            # FR-004 (persona count update)
│   ├── contributing/
│   │   └── _index.md            # FR-005 (persona count update)
│   └── getting-started/
│       ├── common-workflows.md  # FR-006 (Curator in code review table)
│       ├── developer.md         # FR-007, FR-009, FR-010 (gatekeeping + .gitignore docs)
│       └── quick-start.md       # FR-011 (.gitignore mention)
```

**Structure Decision**: No new files or directories. All changes are edits to existing content files in the established Hugo content structure.

## Implementation Strategy

### Phase 1: The Curator — Persona Count Consistency (P1)

**Priority**: Highest. Resolves the blog post ↔ team page contradiction.

**Scope**: 5 pages affected (the-divisor.md, team/\_index.md, contributing/\_index.md, common-workflows.md, the-divisor.md ownership table).

**Approach**:

1. **the-divisor.md — Count updates** (FR-001): Replace all 6 occurrences of "eight" with "nine" in persona count contexts. The word "eight" will still appear in the new gatekeeping section (referring to eight protected value categories), which is correct and distinct from persona counts.

2. **the-divisor.md — Curator section** (FR-002): Add a new subsection under "Content Creation Personas" (or as a new "Triage Persona" section between review and content creation). The Curator is functionally distinct from both review and content creation — it's a triage layer. The section structure should be:
   - Role description: Documentation & Content Pipeline Triage
   - Five audit checklist items (numbered list)
   - Bash access restriction: `gh` CLI only (`gh issue list`, `gh issue create`)
   - Scope boundary: triage, not creation
   - Temperature: 0.2

   **Design decision**: Place The Curator in a new "Triage Persona" section between "Content Creation Personas" and "Exclusive Ownership Boundaries." This mirrors the three-layer model described in the blog post (Review → Triage → Creation) and avoids misclassifying The Curator as a content creation persona.

3. **the-divisor.md — Ownership table** (FR-003): Add a row for The Curator with its exclusive domain: "Documentation & content pipeline triage."

4. **the-divisor.md — Lens count** (implicit): The "Collective Approval" section lists eight lenses. Update to nine and add "documentation triage" to the list.

5. **team/\_index.md** (FR-004): Update the Divisor summary from "eight sub-personas" to "nine sub-personas" and add "The Curator" to the parenthetical list.

6. **contributing/\_index.md** (FR-005): Update the Divisor description from "eight sub-personas" to "nine sub-personas." Add "The Curator" to the persona enumeration. Update "Five review personas" to "Six review personas" to include The Curator in the review-adjacent count (since The Curator runs during code review).

7. **common-workflows.md** (FR-006): Add a Curator row to the code review persona evaluation table:
   ```
   | **Curator** | Documentation gaps, blog/tutorial opportunities, website issue filing |
   ```

### Phase 2: Gatekeeping Value Protection (P2)

**Scope**: 2 pages affected (developer.md, the-divisor.md).

**Approach**:

1. **developer.md — Gatekeeping section** (FR-007): Add a new section titled "Gatekeeping Value Protection" after the "Feedback Loops" section (or within the "Cobalt-Crush Persona" section, since gatekeeping is a behavioral constraint on the developer agent). Content:
   - Brief explanation: certain values are protected from modification by AI agents
   - The eight protected categories (numbered list, sourced from upstream AGENTS.md)
   - The "stop and report" behavior: when an agent cannot meet a gate, it stops and reports the conflict rather than weakening the gate
   - Note: modifying a gate without human authorization is a CRITICAL-severity constitution violation

   **Design decision**: Place this section under "Cobalt-Crush Persona" as a subsection after "Feedback Loops." Gatekeeping is a behavioral constraint that directly affects how Cobalt-Crush operates — it's the reason the developer agent sometimes stops mid-implementation. This placement makes it discoverable in the context where a developer would encounter the behavior.

2. **the-divisor.md — Guard and Adversary audit items** (FR-008): Add brief mentions of the new audit items:
   - Under The Guard section: Add "Gatekeeping Integrity" — detects unauthorized modifications to quality gates
   - Under The Adversary section: Add "Gate Tampering" — detects removal or weakening of CI security controls

### Phase 3: `.gitignore` Management (P2)

**Scope**: 2 pages affected (developer.md, quick-start.md).

**Approach**:

1. **developer.md — `uf init` `.gitignore` docs** (FR-009, FR-010): Add `.gitignore` documentation to the "Project Scaffolding with `uf init`" section:
   - Add `.gitignore` to the "What Gets Deployed" table as a new category row
   - Add a subsection or paragraph explaining the behavior:
     - Append-only: never overwrites existing `.gitignore` content
     - Idempotent: marker comment `# Unbound Force — managed by uf init` prevents duplicate blocks
     - Creates `.gitignore` if it does not exist
     - Two categories of ignored entries: `.uf/` runtime data and legacy tool directories

   **Design decision**: Add `.gitignore` as a row in the existing "What Gets Deployed" table rather than creating a separate subsection. The table already documents what `uf init` deploys, and `.gitignore` is just another file it manages. A brief paragraph after the table explains the append-only/idempotent behavior for readers who want details.

2. **quick-start.md — `.gitignore` mention** (FR-011): Add `.gitignore` management to the `uf setup` bullet list. Brief mention only — the developer guide has the details. Something like adding "`.gitignore` configuration" to the "Project scaffolding" bullet.

### Cross-Cutting Concerns

- **FR-012 (Content Accuracy)**: All content sourced from Dewey-indexed upstream PRs and the local agent definition file. Research.md documents every source.
- **FR-013 (Build Success)**: `npm run build` run after each phase as a checkpoint.
- **FR-014 (Light/Dark Mode)**: Visual verification after all changes. No custom CSS is added, so dark mode should work automatically via Doks theme.
- **FR-015 (No Placeholders)**: No placeholder text, "Coming Soon," or TODO markers in any content.

## Risk Assessment

| Risk                                           | Likelihood | Impact                                  | Mitigation                                                                                                    |
| ---------------------------------------------- | ---------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Missing an "eight" occurrence                  | Low        | Medium — creates internal inconsistency | Grep audit in research.md identified all occurrences. Post-implementation grep verification in quickstart.md. |
| Curator section placement breaks page flow     | Low        | Low — can be repositioned               | Blog post establishes the Review → Triage → Creation model. Follow that structure.                            |
| `.gitignore` documentation overstates behavior | Low        | Medium — violates Content Accuracy      | Source strictly from PR #87 summary and OpenSpec design doc. Do not add details not in the source.            |
| Hugo build failure from malformed Markdown     | Low        | High — blocks deployment                | Run `npm run build` after each phase. Fix immediately.                                                        |

## Dependency Graph

```
Phase 1 (Curator/counts) ──→ Phase 2 (Gatekeeping) ──→ Phase 3 (.gitignore)
```

Phases are sequential because:

- Phase 1 and Phase 2 both modify `the-divisor.md` — concurrent edits risk merge conflicts
- Phase 2 and Phase 3 both modify `developer.md` — same reason
- Phase 1 is highest priority (P1) and should complete first

Within each phase, tasks that touch different files could theoretically be parallelized, but the small scope (~85 lines total) makes sequential execution simpler and less error-prone.

## Next Steps

1. Run `/speckit.tasks` to generate the task breakdown from this plan
2. Run `/speckit.analyze` for cross-artifact consistency
3. Run `/speckit.checklist` for requirement quality validation
4. Commit spec artifacts and push (Spec Commit Gate)
5. Run `/speckit.implement` to execute
