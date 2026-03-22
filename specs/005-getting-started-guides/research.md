# Research: Getting Started Guides

**Branch**: `005-getting-started-guides` | **Date**: 2026-03-22

## R1: Existing Content and Navigation Patterns

**Decision**: Follow the existing flat directory pattern
in `content/docs/getting-started/` with `weight`-based
sidebar ordering.

**Rationale**: The existing section has `_index.md`
(weight: 10) and `quick-start.md` (weight: 20). New pages
use weights 30-70 in 10-step increments, leaving room for
future pages between any pair. This matches the Doks theme's
built-in sidebar discovery (no menu configuration needed).

**Alternatives considered**:

- Subdirectories per role (e.g., `getting-started/developer/`)
  — rejected because the existing pattern is flat and there's
  no need for nested pages within each role guide.
- Adding to `config/_default/menus/menus.en.toml` — rejected
  because Doks auto-discovers pages from directory structure.

## R2: Content Sources Per Guide

**Decision**: Source content from the following upstream
artifacts, adapted for a website audience ("why should I
care" framing per Constitution Principle III).

| Guide            | Primary Sources                                                                                                                                                                           |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Developer        | `unbound-force/unbound-force/AGENTS.md`, `unbound-force/unbound-force/.opencode/agents/cobalt-crush-dev.md`, swarmtools.ai/docs, opencode.ai/docs, `specs/011-doctor-setup/quickstart.md` |
| Tester           | `unbound-force/gaze/README.md`, `unbound-force/unbound-force/.opencode/agents/divisor-*.md`, `unbound-force/unbound-force/AGENTS.md`                                                      |
| Product Owner    | `unbound-force/unbound-force/.opencode/agents/muti-mind-po.md`, `unbound-force/unbound-force/specs/004-muti-mind-architecture/`                                                           |
| Product Manager  | `unbound-force/unbound-force/.opencode/agents/mx-f-coach.md`, `unbound-force/unbound-force/specs/007-mx-f-architecture/`                                                                  |
| Common Workflows | `unbound-force/unbound-force/.opencode/skill/unbound-force-heroes/SKILL.md` (6-stage workflow), `unbound-force/unbound-force/specs/008-swarm-orchestration/spec.md`                       |

**Rationale**: These are the authoritative sources for each
tool's capabilities. Content must be adapted (not copied
verbatim) per Constitution Principle I (Content Accuracy)
and III (Visitor Clarity).

## R3: Frontmatter Convention

**Decision**: Follow the exact frontmatter pattern from
`quick-start.md`:

```yaml
---
title: "Guide Title"
description: "SEO-friendly description (150 chars max)."
lead: "Brief intro shown at top of page."
date: 2026-03-22T00:00:00+00:00
draft: false
weight: NN
toc: true
---
```

**Rationale**: Matches the existing page format exactly.
The `toc: true` generates a table of contents from headings,
which is valuable for long guide pages. The `lead` field
provides a hook visible in the sidebar or page previews.

## R4: Cross-Reference Link Format

**Decision**: Use relative Hugo-style links between guide
pages:

```markdown
[Developer Guide](/docs/getting-started/developer/)
[Common Workflows](/docs/getting-started/common-workflows/)
```

**Rationale**: These are the standard Doks link format.
Hugo resolves them to the correct URLs in the built site.
Relative links ensure they work regardless of base URL
configuration.

## R5: Content Depth and Length

**Decision**: Each guide should be 200-400 lines of
Markdown (roughly 1500-3000 words). Content should be
actionable and workflow-oriented, not reference
documentation. Use H2 for major sections, H3 for
subsections. Include fenced code blocks for CLI commands.

**Rationale**: Long enough to be comprehensive, short
enough to read in one sitting. The existing `quick-start.md`
is 77 lines (very concise). The role guides need more depth
because they cover role-specific workflows, but should not
become exhaustive reference docs (those belong in the
upstream tool repos). Constitution Principle III requires
every page to serve a clear purpose without burying content.
