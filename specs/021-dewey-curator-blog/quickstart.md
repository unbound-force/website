# Quickstart: How Dewey Became a Knowledge Curator Blog Post

**Branch**: `021-dewey-curator-blog` | **Date**: 2026-04-11

## Design Decisions

### D1: Blog Post File Location

**Decision**: `content/blog/dewey-curator-blog.md`

**Rationale**: Follows the existing blog post naming convention (`dewey-vs-karpathy.md`, `dewey-knowledge-retrieval.md`). The `dewey-curator-blog` slug is descriptive and URL-friendly.

### D2: Frontmatter Weight

**Decision**: `weight: 58`

**Rationale**: Places the post between the Karpathy comparison post (weight 60) and the unleash-in-practice post (weight 50) in sidebar ordering. This positions it as a natural follow-up to the comparison post.

### D3: No CLI Syntax in Post Body

**Decision**: All CLI command syntax is linked to docs pages, never inlined in the post.

**Rationale**: Per FR-009 and the Minimal Footprint principle, CLI syntax lives in the docs (updated by spec 020). The blog post describes concepts and architecture. Duplicating syntax creates maintenance burden and drift risk.

### D4: Five-Section Structure

**Decision**: Follow the outline from issue #30 exactly:

1. The Problem: Learnings That Never Compound
2. From Karpathy to Code: Three Key Ideas
3. The Event Sourcing Insight
4. Knowledge Linting: The Self-Healing Index
5. What We Learned Building It

**Rationale**: This structure maps to the spec's user stories — sections 1-2 serve US1 (narrative journey), sections 3-4 serve US2 (architectural insights), and internal links throughout serve US3 (links to docs).

### D5: Karpathy Credit Tone

**Decision**: Frame as "inspired by" not "better than." Acknowledge where Karpathy's approach excels (small curated datasets, personal research). The narrative is "we learned from the comparison and built on those ideas."

**Rationale**: Per FR-011 and the edge cases in the spec, the predecessor post took a balanced approach. This sequel maintains that tone.

## Verification Steps

### V1: Build Verification

```bash
# From repo root
npm run build
```

**Expected**: Zero errors. The new blog post renders in the build output.

### V2: Dev Server Visual Verification

```bash
npm run dev
# Navigate to http://localhost:1313/blog/dewey-curator/
```

**Expected**:

- Post renders with correct title, lead text, and table of contents
- Five sections visible with proper heading hierarchy (H2 for sections)
- Internal links to docs pages are clickable and resolve correctly
- Link to predecessor post (`/blog/dewey-vs-karpathy/`) works

### V3: Dark Mode Verification

```bash
npm run dev
# Toggle dark mode in the Doks theme UI
```

**Expected**: Post renders correctly in both light and dark mode. No contrast issues, no broken styling.

### V4: Internal Link Verification

Check that all internal links in the post resolve:

| Link Target                        | Expected Destination            |
| ---------------------------------- | ------------------------------- |
| `/blog/dewey-vs-karpathy/`         | Predecessor blog post           |
| `/docs/getting-started/knowledge/` | Knowledge getting-started guide |
| `/docs/projects/dewey/`            | Dewey project page              |

### V5: Content Accuracy Spot-Check

Verify key claims against upstream sources:

| Claim in Post                                                        | Source                       |
| -------------------------------------------------------------------- | ---------------------------- |
| "124 new tests across 7 packages"                                    | PR #42 body                  |
| Event sourcing model (append-only log, materialized views)           | PR #42 body                  |
| Category-aware resolution (decisions, patterns, gotchas)             | PR #42 body                  |
| Four lint checks (stale decisions, uncompiled, gaps, contradictions) | PR #42 body, docs pages      |
| Trust tiers (authored/validated/draft)                               | PR #42 body, docs pages      |
| Dual-mode LLM (Ollama for CLI, agent LLM for MCP)                    | PR #42 body (`llm/` package) |

### V6: Word Count Verification

**Expected**: 1200–1800 words (per spec constraint). Count body text only, excluding frontmatter.

### V7: No Time-Sensitive Language

**Expected**: No instances of "just shipped," "this week," "recently," "now available," or similar time-sensitive phrases in the post body. The frontmatter `date` field provides temporal context.

### V8: No Duplicated CLI Syntax

**Expected**: No fenced code blocks containing `dewey compile`, `dewey lint`, or `dewey promote` CLI examples. These live in the docs. The post may mention command names in prose but does not show usage syntax.
