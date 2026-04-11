# Research: Introducing The Curator Blog Post

**Branch**: `022-curator-blog` | **Date**: 2026-04-11

## R1: Upstream Source Material — PR #89

**Question**: What exactly shipped in PR #89 that the blog post needs to describe?

**Source**: `gh pr view 89 --repo unbound-force/unbound-force` (verified 2026-04-11)

**Findings**:

PR #89 title: "feat: add Divisor Curator agent for documentation & content pipeline triage (Spec 026)"
State: MERGED (2026-04-11T19:53:18Z)

- **The Curator agent** (`divisor-curator.md`, ~250 lines):
  - Role: Documentation & content pipeline triage
  - Temperature: 0.2 (judgment-based decisions)
  - Tools: `read: true, write: false, edit: false, bash: true`
  - Bash restriction: Only `gh issue list` and `gh issue create` against `unbound-force/website`
  - The Adversary's "Gate Tampering" check enforces the bash restriction
  - Operates in two modes: Code Review Mode (default) and Spec Review Mode

- **Five audit checklist items**:
  1. Documentation Gap Detection — checks if AGENTS.md/README were updated for user-facing changes
  2. Website Documentation Issue Check — auto-files `docs`-labeled issues in `unbound-force/website`
  3. Duplicate Issue Check — searches existing open issues before filing new ones
  4. Blog Opportunity Identification — files `blog`-labeled issues for significant new capabilities
  5. Tutorial Opportunity Identification — files `tutorial`-labeled issues for new workflows

- **Guard Enhancement**: Added `#### 6. Documentation Completeness` to the Guard's Code Review audit

- **Scale**: 1 new file + scaffold copy, 4 modified files, AGENTS.md updated, asset count 54→55, Divisor agents 8→9

- **Website repo labels created**: `docs`, `blog`, `tutorial`

**Decision**: Use PR #89 body as the authoritative source for all technical claims about The Curator. Cross-reference with the local agent definition file for implementation details.

## R2: The Curator Agent Definition — Key Design Details

**Question**: What are the specific design decisions in The Curator's agent file that make good blog material?

**Source**: `.opencode/agents/divisor-curator.md` (local copy, committed)

**Findings**:

### Bash Access Restriction

The Curator is the first Divisor persona with `bash: true` in its frontmatter. All other Divisor agents have `bash: false`. The restriction is enforced at two levels:

1. **Self-documented**: The agent file's "Bash Access Restriction" section explicitly states only two operations are permitted: `gh issue list` and `gh issue create`
2. **Cross-agent enforcement**: "The Adversary agent's 'Gate Tampering' check covers this" — meaning another Divisor persona actively monitors for bash misuse

This is a trust architecture pattern: grant the minimum capability needed, then have a peer agent verify compliance.

### User-Facing Change Detection Heuristic

The Curator classifies files as user-facing or internal based on path patterns:

- **User-facing paths** (trigger documentation checks): `cmd/`, `.opencode/agents/`, `.opencode/command/`, `.opencode/skill/`, `internal/scaffold/`, `AGENTS.md`, `README.md`, `unbound-force.md`
- **Internal paths** (skip documentation checks): `internal/` (excluding scaffold), `*_test.go`, `.github/`, `specs/`, `openspec/`

This is heuristic file-path matching, not semantic understanding. The Curator does not read the content of changed files to determine whether documentation is needed — it uses path patterns as a proxy.

### Triage-Only Scope (Out of Scope Section)

The agent file explicitly lists what The Curator does NOT do:

- Writing documentation → The Scribe
- Writing blog posts → The Herald
- Writing PR communications → The Envoy
- Code quality → The Architect
- Security → The Adversary
- Test quality → The Tester
- Intent drift → The Guard
- Operational readiness → The SRE

The Curator identifies **what** needs documenting and files tracking issues. The content agents and development team create the actual content.

### Internal-Only Change Exemption

If all changed files fall into internal-only paths, The Curator produces no findings and APPROVEs. This prevents noise — a refactoring PR that touches only `internal/` code should not trigger documentation gap warnings.

**Decision**: The bash restriction, heuristic detection, and triage/creation separation are the three design insights worth highlighting in the "Interesting Bits" section. The internal-only exemption is a supporting detail.

## R3: The Bootstrap Paradox

**Question**: What is the bootstrap paradox and how was it handled in practice?

**Source**: PR #89 context, agent definition file

**Findings**:

The bootstrap paradox: The Curator cannot review the PR that creates it, because it does not exist until that PR merges.

PR #89 adds `divisor-curator.md` to the repository. The review council discovers Divisor personas by scanning for `divisor-*.md` files in `.opencode/agents/`. During the review of PR #89, the file `divisor-curator.md` exists in the diff but is not yet deployed — the review council runs against the current state of the repository, not the proposed state.

This means:

1. PR #89 was reviewed by the existing 8 personas (Guard, Architect, Adversary, SRE, Tester, Scribe, Herald, Envoy)
2. The Curator could not participate in its own review because it did not exist yet
3. After PR #89 merged, The Curator became active for all subsequent PRs
4. The first PR that The Curator actually reviewed was the one after its own creation

This is a genuine self-referential problem in self-modifying agent systems. The Curator's first act of documentation governance could not apply to the documentation of its own creation.

**Decision**: The bootstrap paradox is the most narratively interesting design insight. Frame it as a real engineering problem, not a hypothetical — describe what actually happened during PR #89's review cycle.

## R4: Existing Divisor Team Page — Current State

**Question**: What does the Divisor team page currently say, and how does The Curator change the count?

**Source**: `content/docs/team/the-divisor.md` (local)

**Findings**:

The current team page describes:

- **Title/description**: "eight sub-personas for code review and content creation"
- **5 review personas**: Guard, Architect, Adversary, SRE (Operator), Tester
- **3 content creation personas**: Scribe, Herald, Envoy
- **Total**: 8 personas

The Curator is the 9th persona. It is neither a pure review persona nor a pure content creation persona — it is a triage persona that bridges the gap. It reviews code for documentation impact and files issues for the content agents to act on.

The blog post should state: The Curator is the 9th Divisor persona, joining the existing five review personas and three content creation personas. It occupies a new category — documentation triage — that connects the review and content creation sides of the council.

**Decision**: State "9th persona" clearly. Note the new category (triage) that The Curator occupies. Do not modify the team page — that is out of scope per the spec.

## R5: Triage vs Creation Separation — The Content Pipeline

**Question**: How does the triage/creation separation work across the Divisor council?

**Source**: Agent definition files (Curator, Scribe, Herald, Envoy), PR #84 (content agents)

**Findings**:

The Divisor council now has three functional layers:

1. **Review** (5 personas): Guard, Architect, Adversary, SRE, Tester — evaluate code quality
2. **Triage** (1 persona): Curator — identifies documentation gaps and files tracking issues
3. **Creation** (3 personas): Scribe, Herald, Envoy — produce documentation, blog posts, and communications

The flow is:

1. A PR changes user-facing behavior (e.g., new CLI command)
2. The Curator detects the documentation gap during review
3. The Curator files a GitHub issue in `unbound-force/website` with the appropriate label (`docs`, `blog`, or `tutorial`)
4. The content agents (Scribe, Herald, Envoy) pick up those issues and create the content

This separation exists because detection and creation require different capabilities:

- Detection needs `read` + `bash` (to read diffs and file issues)
- Creation needs `write` + `edit` (to produce content files)
- Combining both in one agent would violate the principle of least privilege

**Decision**: The triage/creation separation is a clean separation of concerns story. Frame it as: "The Curator files the ticket. The Scribe writes the docs. The Herald writes the blog post. No single agent does everything."

## R6: Blog Post Frontmatter Pattern

**Question**: What frontmatter format and weight should the new post use?

**Source**: Existing blog posts in `content/blog/`

**Findings**:

Current blog post weights (lower = higher in sidebar):

| Weight | Post                       | Slug                      |
| ------ | -------------------------- | ------------------------- |
| 58     | Dewey Curator blog         | dewey-curator             |
| 60     | Librarian vs Index         | dewey-vs-karpathy         |
| 70     | AI Agent Doesn't Read Docs | dewey-knowledge-retrieval |
| 80     | From Spec to Demo          | unleash-in-practice       |
| 90     | Gaze in Practice           | gaze-in-practice          |
| 100    | Why Contract Coverage      | why-contract-coverage     |

All posts use:

```yaml
categories: ["Engineering"]
contributors: ["Unbound Force"]
```

Tags vary by topic. Slugs are descriptive and URL-friendly.

**Decision**: Use `weight: 56` to place the new post above the Dewey Curator blog post (58) in sidebar ordering — it is the most recent post. Use `slug: "introducing-the-curator"` for a descriptive URL. Tags: `["divisor", "documentation", "code-review", "agents"]`. Categories: `["Engineering"]`.

## R7: Predecessor Blog Post Pattern — Spec 021

**Question**: What pattern did the Dewey Curator blog post (spec 021) establish that this post should follow?

**Source**: `specs/021-dewey-curator-blog/plan.md`, `content/blog/dewey-curator-blog.md`

**Findings**:

Spec 021 established these patterns:

1. **Narrative structure**: Problem → Technical details → Lessons learned. Not a feature announcement.
2. **No CLI duplication**: All CLI syntax linked to docs pages, never inlined in the post.
3. **Internal links**: Link to getting-started guides and project pages for readers who want to try features.
4. **Honest scoping**: "We are not overclaiming the maturity of these features" — acknowledge limitations.
5. **Word count**: 1200-1800 words target.
6. **No time-sensitive language**: No "just shipped," "this week," "recently."
7. **Source attribution**: Every factual claim traceable to a specific PR or spec.

**Decision**: Follow the same pattern. The three-section structure (Problem → Solution → Interesting Bits) is this post's equivalent of spec 021's five-section structure. Same rules apply: no CLI duplication, honest scoping, source attribution.

## R8: Heuristic Limitations — Honest Scoping

**Question**: What are The Curator's actual limitations that the post must acknowledge?

**Source**: Agent definition file, spec edge cases

**Findings**:

The Curator's detection is heuristic, not semantic:

1. **File-path matching**: The Curator classifies files as user-facing or internal based on path patterns. A change to `cmd/foo.go` triggers documentation checks. A change to `internal/bar.go` does not. But a change to `internal/scaffold/baz.go` does (because scaffold output affects what `uf init` deploys).

2. **No content analysis**: The Curator does not read the content of changed files to determine whether documentation is actually needed. It uses path patterns as a proxy. A cosmetic refactoring of a CLI command (no behavior change) would still trigger a documentation gap warning if the file is in a user-facing path.

3. **No semantic understanding**: The Curator does not understand whether existing documentation already covers the change. It checks whether documentation files were modified in the same PR, not whether the existing docs are already accurate.

4. **Duplicate detection is keyword-based**: The `gh issue list --search` command uses keyword matching, not semantic similarity. A documentation gap issue titled "docs: update CLI reference for new flag" might not match a search for "CLI documentation update."

**Decision**: The post must acknowledge these limitations per FR-013 and the spec's edge cases. Frame as: "The Curator uses heuristic file-path matching, not semantic understanding. It is a first-pass filter, not an oracle."
