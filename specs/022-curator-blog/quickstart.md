# Quickstart: Introducing The Curator Blog Post

**Branch**: `022-curator-blog` | **Date**: 2026-04-11

## Design Decisions

### D1: Blog Post File Location

**Decision**: `content/blog/curator-blog.md`

**Rationale**: Follows the existing blog post naming convention. The filename `curator-blog` is descriptive and distinguishes this post from the Dewey Curator blog post (`dewey-curator-blog.md`). The slug `introducing-the-curator` provides a clean, descriptive URL.

### D2: Frontmatter Weight

**Decision**: `weight: 56`

**Rationale**: Places the post above the Dewey Curator blog post (weight 58) in sidebar ordering, making it the most recent post. Lower weight = higher position in the Doks sidebar.

### D3: Three-Section Structure

**Decision**: Follow the outline from issue #36:

1. The Problem — Documentation Drift
2. The Solution — A Dedicated Triage Agent
3. The Interesting Bits — Bash Access, Bootstrap Paradox, Triage vs Creation

**Rationale**: This structure maps to the spec's user stories — section 1 serves US1 (documentation drift problem and Curator's role), section 2 continues US1 (how The Curator works), and section 3 serves US2 (design insights for technical readers). Internal links throughout serve US3.

### D4: No Agent Configuration or CLI Syntax in Post Body

**Decision**: All agent configuration syntax, `gh` CLI commands, and heuristic rules are linked to docs pages, never inlined in the post.

**Rationale**: Per FR-009 and the Minimal Footprint principle, implementation details that will drift (prompt contents, heuristic rules, CLI syntax) live in the agent definition file and docs. The blog post describes concepts and design decisions. Duplicating syntax creates maintenance burden and drift risk — the exact problem The Curator is designed to catch.

### D5: Honest Limitations Acknowledgment

**Decision**: Explicitly state that The Curator uses heuristic file-path matching, not semantic understanding. Acknowledge it as a first-pass filter.

**Rationale**: Per FR-013 and the spec's edge cases, the post must not overclaim The Curator's intelligence. The Curator does not understand whether documentation is actually needed — it uses path patterns as a proxy. This honesty is consistent with the tone established in spec 021 ("We are not overclaiming the maturity of these features").

### D6: Bootstrap Paradox as Narrative Hook

**Decision**: Present the bootstrap paradox as a real engineering story from PR #89, not a hypothetical scenario.

**Rationale**: Per FR-007, the post must describe how the bootstrap paradox was handled in practice. PR #89 was reviewed by the existing 8 personas. The Curator could not participate in its own review. This is a concrete, verifiable fact that makes the design insight tangible.

### D7: Persona Count

**Decision**: State "9th Divisor persona" explicitly, noting it joins 5 review + 3 content creation personas.

**Rationale**: Per FR-011 and the spec's edge cases, the persona count must be accurate. The existing team page says 8. The Curator makes 9. The post states the updated count clearly without modifying the team page (out of scope).

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
# Navigate to http://localhost:1313/blog/introducing-the-curator/
```

**Expected**:

- Post renders with correct title, lead text, and table of contents
- Three sections visible with proper heading hierarchy (H2 for sections, H3 for subsections)
- Internal links to docs pages are clickable and resolve correctly

### V3: Dark Mode Verification

```bash
npm run dev
# Toggle dark mode in the Doks theme UI
```

**Expected**: Post renders correctly in both light and dark mode. No contrast issues, no broken styling.

### V4: Internal Link Verification

Check that all internal links in the post resolve:

| Link Target                               | Expected Destination            |
| ----------------------------------------- | ------------------------------- |
| `/docs/team/the-divisor/`                 | Divisor team page               |
| `/docs/getting-started/common-workflows/` | Common workflows (code review)  |
| `/docs/getting-started/developer/`        | Developer getting-started guide |

### V5: Content Accuracy Spot-Check

Verify key claims against upstream sources:

| Claim in Post                                              | Source                                                    |
| ---------------------------------------------------------- | --------------------------------------------------------- |
| The Curator is the 9th Divisor persona                     | PR #89 body (8→9 count)                                   |
| First Divisor agent with bash access                       | Agent definition frontmatter                              |
| Bash restricted to `gh issue list` and `gh issue create`   | Agent definition "Bash Access" section                    |
| Five audit checklist items                                 | Agent definition "Audit Checklist"                        |
| Triage only — does not write docs, blog, or tutorials      | Agent definition "Out of Scope" section                   |
| Heuristic file-path matching, not semantic understanding   | Agent definition "User-Facing Change Detection Heuristic" |
| The Adversary enforces bash restriction                    | Agent definition "Bash Access" section                    |
| PR #89 reviewed by existing 8 personas (bootstrap paradox) | PR #89 review history                                     |

### V6: Word Count Verification

**Expected**: 1200-1800 words (per spec constraint). Count body text only, excluding frontmatter.

### V7: No Time-Sensitive Language

**Expected**: No instances of "just shipped," "this week," "recently," "now available," or similar time-sensitive phrases in the post body. The frontmatter `date` field provides temporal context.

### V8: No Duplicated Configuration or CLI Syntax

**Expected**: No fenced code blocks containing agent frontmatter YAML, `gh issue create` commands, or heuristic path patterns. These live in the agent definition file and docs. The post may mention concepts in prose but does not show implementation syntax.

### V9: No Overclaiming

**Expected**: The post explicitly acknowledges The Curator's heuristic limitations. No claims of "semantic understanding," "intelligent detection," or similar overclaims. The Curator is described as a first-pass filter using file-path matching.
