# Implementation Plan: How Dewey Became a Knowledge Curator Blog Post

**Branch**: `021-dewey-curator-blog` | **Date**: 2026-04-11 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/021-dewey-curator-blog/spec.md`

## Summary

Create a blog post that tells the engineering story of how Dewey v3.0.0 shipped knowledge compilation, linting, and trust tiers — transforming Dewey from a passive index into an active knowledge curator. The post is a narrative sequel to the existing "The Librarian vs The Index" blog post (spec 019), closing the loop on speculative ideas that became real features. Single new file: `content/blog/dewey-curator-blog.md`.

## Technical Context

**Language/Version**: Markdown (Hugo content file)  
**Primary Dependencies**: Hugo (via @thulite), Doks theme (`@thulite/doks-core`)  
**Storage**: N/A (static Markdown file)  
**Testing**: `npm run build` (zero errors), visual verification in `npm run dev`  
**Target Platform**: GitHub Pages (static site)  
**Project Type**: Static site content (blog post)  
**Performance Goals**: N/A (static content)  
**Constraints**: 1200–1800 words, five-section outline per issue #30  
**Scale/Scope**: 1 new Markdown file

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### I. Content Accuracy — PASS

All factual claims in the blog post are derived from verified upstream sources:

- **Dewey PR #42** (8,598 additions, 152 deletions across 39 files): "feat: add knowledge compilation, temporal intelligence, linting, and trust tiers." Verified via `gh pr view 42 --repo unbound-force/dewey`.
- **Spec 013-knowledge-compile**: 6 user stories, 28 FRs, 40 tasks (all complete), 124 new tests across 7 packages.
- **Existing website docs** (updated by spec 020): Knowledge lifecycle, trust tiers, compile/lint/promote commands documented on the [knowledge getting-started guide](/docs/getting-started/knowledge/) and [Dewey project page](/docs/projects/dewey/).
- **Predecessor blog post** (spec 019): "The Librarian vs The Index" at `content/blog/dewey-vs-karpathy.md`, which explicitly speculated about `dewey compile` and contamination separation.

No capabilities are fabricated. The post describes what was built, sourced from the PR and spec artifacts.

### II. Minimal Footprint — PASS

- 1 new Markdown file. No custom HTML, CSS, JavaScript, or template overrides.
- No new dependencies added to `package.json`.
- Blog section already exists (5 published posts). No navigation or section setup needed.
- CLI syntax and API details are linked to existing docs pages, not duplicated in the post.

### III. Visitor Clarity — PASS

- The post follows the established blog format (same frontmatter structure, categories, tags as existing posts).
- The five-section narrative structure (Problem → Karpathy to Code → Event Sourcing → Knowledge Linting → Lessons Learned) provides a clear reading path.
- Readers familiar with the predecessor post get narrative continuity; readers without prior context get a self-contained engineering story.
- Internal links to docs pages provide a clear path for readers who want to try the features.

## Project Structure

### Documentation (this feature)

```text
specs/021-dewey-curator-blog/
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
├── dewey-vs-karpathy.md           # Predecessor post (existing, not modified)
├── dewey-knowledge-retrieval.md   # Existing post (not modified)
├── dewey-curator-blog.md          # NEW — this spec's deliverable
├── gaze-in-practice.md            # Existing post (not modified)
├── unleash-in-practice.md         # Existing post (not modified)
└── why-contract-coverage.md       # Existing post (not modified)
```

**Structure Decision**: Single new Markdown file in the existing `content/blog/` directory. No structural changes to the site.

## File Change Matrix

| File                                 | Change          | Rationale                         |
| ------------------------------------ | --------------- | --------------------------------- |
| `content/blog/dewey-curator-blog.md` | NEW — blog post | The sole deliverable of this spec |

## Constitution Re-Check (Post-Design)

### I. Content Accuracy — PASS (confirmed)

Research phase (see [research.md](research.md)) verified all source material:

- PR #42 body confirms: 4 interconnected capabilities, event sourcing model, 124 tests across 7 packages, schema v1→v2 migration, 3 new MCP tools (#45-47), 3 new CLI commands, new `llm/` package.
- Category-aware resolution confirmed: decisions=temporal merge, patterns=accumulate, gotchas=dedup.
- Dual-mode LLM pattern confirmed: `OllamaSynthesizer` in new `llm/` package for CLI compilation; agent's own LLM for MCP tool calls.
- Trust tiers confirmed: authored/validated/draft via new `tier` column, `dewey promote` for tier transitions.

### II. Minimal Footprint — PASS (confirmed)

No design decisions introduced any additional files, dependencies, or custom elements beyond the single Markdown blog post.

### III. Visitor Clarity — PASS (confirmed)

The five-section outline maps directly to the spec's user stories:

- Sections 1-2 serve US1 (narrative journey from comparison to implementation)
- Sections 3-4 serve US2 (architectural insights for technical readers)
- Internal links throughout serve US3 (links to updated documentation)

## Complexity Tracking

No constitution violations. No complexity justification needed.
