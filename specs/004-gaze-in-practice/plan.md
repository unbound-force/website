# Implementation Plan: "Gaze in Practice" Blog Post and Docs Sample Output

**Branch**: `004-gaze-in-practice` | **Date**: 2026-03-02 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `/specs/004-gaze-in-practice/spec.md`

## Summary

Add a new blog post ("Gaze in Practice: A Real-World Quality Report Walkthrough") that walks through real Gaze output from the gcal-organizer project, section by section, with interpretive narrative. Additionally, add a condensed "Sample Output" section to the existing Gaze project docs page showing the Health Assessment scorecard and Top 5 Worst CRAP Scores table, with a link to the full blog walkthrough.

Both deliverables use real data from `temp/tmp.md` — no fabricated numbers. The blog post is standard Markdown content; the docs change is a new section inserted into an existing file. No custom templates, layouts, or CSS are needed.

## Technical Context

**Language/Version**: Hugo (via Thulite/npm), Markdown content
**Primary Dependencies**: `@thulite/doks-core ^1.8.3` (theme), Hugo SSG
**Storage**: N/A (static Markdown files)
**Testing**: Manual — `npm run build` must succeed; visual verification via `npm run dev`
**Target Platform**: GitHub Pages (static site)
**Project Type**: Static documentation website
**Performance Goals**: N/A
**Constraints**: Content must follow AGENTS.md conventions (YAML frontmatter, H2 body start, fenced code blocks with language IDs)
**Scale/Scope**: 1 new file (blog post), 1 modified file (docs page)

## Constitution Check

*GATE: Must pass before implementation.*

| Principle | Status | Evidence |
|-----------|--------|----------|
| I. Content Accuracy | PASS | All data is reproduced verbatim from actual Gaze output (`temp/tmp.md`). No features are fabricated. Warnings about insufficient data are included honestly. Gaze version (v1.2.3) and date (2026-03-02) are identified so content is understood as a point-in-time snapshot. |
| II. Minimal Footprint | PASS | No custom templates, layouts, CSS, or dependencies. One new Markdown file and one section addition to an existing Markdown file. Uses standard Doks table and code block rendering. |
| III. Visitor Clarity | PASS | Blog post explains each section of real output for developers evaluating Gaze. Docs page gets a quick visual preview. Cross-links connect the blog post, docs page, and existing "Why Contract Coverage" article. Blog post is reachable in two clicks from homepage (navbar → Blog → article, or "Latest Articles" → article). |

No violations. No complexity tracking needed.

## Project Structure

### Documentation (this feature)

```text
specs/004-gaze-in-practice/
├── spec.md              # Feature specification
├── plan.md              # This file
└── tasks.md             # Task list (created next)
```

### Source Code (content changes)

```text
website/content/
├── blog/
│   ├── why-contract-coverage.md    # Existing — NOT modified
│   └── gaze-in-practice.md         # NEW — full walkthrough blog post
└── docs/
    └── projects/
        └── gaze.md                 # MODIFIED — new "Sample Output" section added
```

**Structure Decision**: No structural changes. One new content file and one section insertion into an existing content file. Both follow established Markdown and frontmatter conventions.

## Implementation Approach

### Phase 1: Blog Post (`content/blog/gaze-in-practice.md`)

Create the blog post with the following structure:

1. **Frontmatter** — title, description, lead, slug (`gaze-in-practice`), date (2026-03-02), categories (`["Engineering"]`), tags (`["gaze", "testing", "go", "quality"]`), contributors, weight 90 (lower than existing post's 100 so it sorts as newer).

2. **Introduction** — Establish context: what project was analyzed (gcal-organizer, a Go-based Google Calendar organizer tool), why it's a good test subject (real side project with real gaps, not a contrived demo), what the reader will learn (how to read and act on each section of a Gaze report). Link to "Why Contract Coverage" for conceptual background.

3. **Report walkthrough sections** — For each section of the Gaze output:
   - Show the actual output (tables, metrics) in a fenced code block or Markdown table
   - Follow with 2-4 paragraphs of interpretation: what the numbers mean, what stands out, what action a developer would take

   Sections in order:
   - CRAP Summary (aggregate metrics)
   - Top 5 Worst CRAP Scores (specific functions)
   - GazeCRAP Quadrant Distribution (Q1-Q4 breakdown)
   - Quality Summary (including the "0 tests" warning)
   - Classification Summary (including the insufficient data warning)
   - Overall Health Assessment (scorecard + prioritized recommendations)

4. **Wrap-up** — Brief summary of what the report tells us about gcal-organizer's test health and how a developer would prioritize next steps. Link to Gaze project docs page for setup instructions.

### Phase 2: Docs Page Update (`content/docs/projects/gaze.md`)

Insert a new `## Sample Output` section after the "Three Modes" subsection (after line 121 in current file) and before "Architecture" (line 127). Contents:

1. One-sentence intro: "Here's what a full Gaze report looks like on a real Go project."
2. Project identification (gcal-organizer, Gaze v1.2.3)
3. Health Assessment scorecard table (5 rows: CRAPload, GazeCRAPload, Avg Line Coverage, Contract Coverage, Complexity)
4. Top 5 Worst CRAP Scores table
5. One-line link to blog post: "For a section-by-section walkthrough of the full report, see [Gaze in Practice](/blog/gaze-in-practice/)."

### Phase 3: Verification

1. `npm run build` — must complete with zero errors
2. `npm run dev` — visual verification:
   - Blog index lists both posts, "Gaze in Practice" appears first
   - Blog post renders all tables and code blocks correctly
   - Homepage "Latest Articles" shows "Gaze in Practice" as most recent
   - Gaze docs page shows new section with correct tables
   - All cross-links work
   - Both light and dark mode render correctly
