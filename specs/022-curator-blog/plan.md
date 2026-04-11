# Implementation Plan: Introducing The Curator Blog Post

**Branch**: `022-curator-blog` | **Date**: 2026-04-11 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/022-curator-blog/spec.md`

## Summary

Create a blog post introducing The Curator — the 9th Divisor persona and the first with bash access — that tells the story of why documentation drift is a persistent problem in AI agent workflows and how a dedicated triage agent addresses it. The post follows the three-section structure from issue #36: (1) The Problem — documentation drift, (2) The Solution — a dedicated triage agent, (3) The Interesting Bits — bash access restriction, bootstrap paradox, triage vs creation separation. Single new file: `content/blog/curator-blog.md`.

## Technical Context

**Language/Version**: Markdown (Hugo content file)
**Primary Dependencies**: Hugo (via @thulite), Doks theme (`@thulite/doks-core`)
**Storage**: N/A (static Markdown file)
**Testing**: `npm run build` (zero errors), visual verification in `npm run dev`
**Target Platform**: GitHub Pages (static site)
**Project Type**: Static site content (blog post)
**Performance Goals**: N/A (static content)
**Constraints**: 1200-1800 words, three-section outline per issue #36
**Scale/Scope**: 1 new Markdown file

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### I. Content Accuracy — PASS

All factual claims in the blog post are derived from verified upstream sources:

- **Upstream PR #89** (unbound-force/unbound-force): "feat: add Divisor Curator agent for documentation & content pipeline triage (Spec 026)." Merged 2026-04-11. Verified via `gh pr view 89 --repo unbound-force/unbound-force`.
- **PR #89 body confirms**: 1 new file (`divisor-curator.md`, ~250 lines), 4 modified files, 5 audit checklist items, bash restricted to `gh issue list` and `gh issue create` only, temperature 0.2, Divisor agent count 8→9, asset count 54→55.
- **Spec 026-documentation-curation**: Full Speckit spec pipeline (spec, plan, tasks, research, data-model, quickstart, contracts, checklists) in the upstream repo.
- **The Curator agent definition** (`.opencode/agents/divisor-curator.md`): Already committed locally. Confirms bash restriction, five audit checklist items, triage-only scope, out-of-scope boundaries.
- **Existing Divisor team page** (`content/docs/team/the-divisor.md`): Currently documents 8 personas (5 review + 3 content creation). The Curator is the 9th.

No capabilities are fabricated. The post describes what was built, sourced from PR #89 and the agent definition file.

### II. Minimal Footprint — PASS

- 1 new Markdown file. No custom HTML, CSS, JavaScript, or template overrides.
- No new dependencies added to `package.json`.
- Blog section already exists (6 published posts). No navigation or section setup needed.
- Agent configuration syntax and `gh` CLI commands are linked to docs, not duplicated in the post.

### III. Visitor Clarity — PASS

- The post follows the established blog format (same frontmatter structure, categories, tags as existing posts).
- The three-section narrative structure (Problem → Solution → Interesting Bits) provides a clear reading path.
- Readers unfamiliar with Unbound Force get a self-contained explanation of the documentation drift problem.
- Internal links to the Divisor team page and getting-started guides provide a clear path for readers who want to learn more.

## Project Structure

### Documentation (this feature)

```text
specs/022-curator-blog/
├── plan.md              # This file
├── research.md          # Phase 0 output — upstream source material
├── quickstart.md        # Phase 1 output — verification steps
└── tasks.md             # Phase 2 output (/speckit.tasks command)
```

No `data-model.md` or `contracts/` — this is a single blog post file with no data model or API contracts.

### Source Code (repository root)

```text
content/blog/
├── _index.md                      # Blog section index (existing)
├── dewey-curator-blog.md          # Existing post (not modified)
├── dewey-vs-karpathy.md           # Existing post (not modified)
├── dewey-knowledge-retrieval.md   # Existing post (not modified)
├── gaze-in-practice.md            # Existing post (not modified)
├── unleash-in-practice.md         # Existing post (not modified)
├── why-contract-coverage.md       # Existing post (not modified)
└── curator-blog.md                # NEW — this spec's deliverable
```

**Structure Decision**: Single new Markdown file in the existing `content/blog/` directory. No structural changes to the site.

## File Change Matrix

| File                           | Change          | Rationale                         |
| ------------------------------ | --------------- | --------------------------------- |
| `content/blog/curator-blog.md` | NEW — blog post | The sole deliverable of this spec |

## Constitution Re-Check (Post-Design)

### I. Content Accuracy — PASS (confirmed)

Research phase (see [research.md](research.md)) verified all source material:

- PR #89 body confirms: The Curator is a documentation & content pipeline triage agent, temperature 0.2, bash restricted to `gh issue list` and `gh issue create` only, 5 audit checklist items, Divisor agent count 8→9.
- Bootstrap paradox confirmed: The Curator cannot review the PR that creates it because it does not exist until that PR merges. PR #89 was reviewed by the existing 8 personas without Curator participation.
- Triage/creation separation confirmed: The Curator's "Out of Scope" section explicitly lists The Scribe (tech docs), The Herald (blog), and The Envoy (comms) as the content creators. The Curator identifies gaps and files issues only.
- Bash restriction confirmed: Agent definition frontmatter shows `bash: true` with body text restricting usage to exactly two operations (`gh issue list` and `gh issue create`). The Adversary's "Gate Tampering" check enforces this.
- Heuristic detection confirmed: The Curator uses file-path pattern matching (user-facing paths vs internal paths) to determine whether documentation checks apply, not semantic understanding of content.

### II. Minimal Footprint — PASS (confirmed)

No design decisions introduced any additional files, dependencies, or custom elements beyond the single Markdown blog post.

### III. Visitor Clarity — PASS (confirmed)

The three-section outline maps directly to the spec's user stories:

- Section 1 (The Problem) serves US1 — explains documentation drift and why it matters
- Section 2 (The Solution) serves US1 — explains The Curator's role and triage model
- Section 3 (The Interesting Bits) serves US2 — three design insights for technical readers
- Internal links throughout serve US3 — links to Divisor team page and docs

## Complexity Tracking

No constitution violations. No complexity justification needed.
