## Context

The website has 14 published blog posts in `content/blog/` and no tutorials section. There are 14 GitHub issues with detailed content outlines ready for implementation — 9 blog posts and 5 tutorials. The proposal (constitution alignment: all N/A — static content only) establishes the scope.

The blog section uses standard Hugo/Doks Markdown with YAML frontmatter. Each post has a `weight` for ordering (lower = newer/higher position), `slug` for URL, `categories`, `tags`, and `contributors` fields. Weights currently range from 1 (section index) to 100.

There is no tutorials section. Creating one requires a new content directory, section index, and navigation entry.

## Goals / Non-Goals

### Goals
- Produce 9 blog posts and 5 tutorials as standard Hugo Markdown files
- Create the tutorials section infrastructure (`_index.md`, navigation entry)
- Follow established blog frontmatter conventions exactly
- Source all content from the detailed outlines in the referenced GitHub issues
- Close all 14 issues upon merge

### Non-Goals
- Custom layouts or shortcodes for tutorials — standard Doks Markdown rendering is sufficient
- Code samples that execute — all code blocks are illustrative/configuration examples
- Extensive cross-linking between blog posts and tutorials is deferred to a follow-up PR — each page stands alone in this batch. Natural pairings (e.g., blog #69 ↔ tutorial #70, blog #150 ↔ tutorial #151) SHOULD include a brief "Learn More" or "Try It" link to the companion page where one exists
- Images or diagrams — text-only content for this batch
- Restructuring existing blog posts or their weights

## Decisions

### D1: Tutorials live under `content/docs/tutorials/`

Tutorials are documentation (step-by-step guides), not blog posts (opinion/analysis pieces). Placing them under `content/docs/` gives them sidebar navigation, table of contents, and the standard docs layout — all appropriate for instructional content. The Doks theme handles this automatically with no custom templates.

### D2: Tutorial navigation at weight 37

The docs sidebar currently has: Getting Started (10), Projects (20), The Team (30), Reference (35), Changelog (38), Contributing (40). Tutorials fit between Reference and Changelog at weight 37, positioning them after reference material but before changelog and contributing — a natural reading order for someone learning the tools.

### D3: Blog post weight allocation

New blog posts will use weights 101-109 (ascending by issue number), placing them after all existing posts. The `_index.md` weight of 1 controls the section itself; individual post weights control ordering within the section. Higher weight = further down the list. Since blog posts are typically consumed newest-first and the blog page already sorts by date, weight is a secondary concern — but consistent allocation prevents future conflicts.

### D4: Content sourcing from GitHub issues only

Each issue contains a structured outline with sections, key points, and audience framing. Content will be written by expanding these outlines into full prose. During implementation, agents SHOULD use Dewey MCP tools (FA-003) or the Read tool (FA-004) to verify specific claims (version numbers, command names, configuration keys) against upstream repositories when practical. The issue outlines provide the structure and framing; upstream verification confirms accuracy of technical details.

### D5: Slug naming convention

Blog slugs follow the established pattern: lowercase, hyphenated, descriptive. Examples from existing posts: `gaze-in-practice`, `sandbox-isolation`, `convention-packs`. New posts will follow the same pattern. Tutorial URLs are determined by their directory/file path under `content/docs/tutorials/`.

### D6: Frontmatter date

All posts and tutorials in this batch will use the same date (2026-08-23) since they are being written as a batch. This is the implementation date, not the date of the feature being described.

## Risks / Trade-offs

### R1: Content accuracy without upstream verification

**Risk**: Blog posts describe features from Gaze, Dewey, Pinkman, and Unbound Force. The GitHub issue outlines may reference implementation details that have since changed.

**Mitigation**: The issues were filed by the team with accurate, detailed outlines. For this static content batch, the outlines are treated as authoritative. Any inaccuracies can be corrected in follow-up PRs after subject-matter review.

### R2: Large PR size

**Risk**: 16 new files in a single PR may be difficult to review.

**Mitigation**: All files are independent Markdown with no cross-dependencies. Reviewers can evaluate each file individually. The files share no code or logic — a problem in one post does not affect others.

### R3: Tutorial section may need refinement

**Risk**: The tutorials section is new. Navigation placement (weight 37) and section description may need adjustment after seeing it in context.

**Mitigation**: Both are trivially adjustable — weight is a single integer in TOML, and the section description is a line in `_index.md`. Visual verification during `npm run dev` will catch layout issues before merge.
