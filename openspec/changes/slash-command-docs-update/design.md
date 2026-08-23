## Context

The Unbound Force toolchain shipped a namespace migration moving all hero-specific commands from flat names to the `uf.*` namespace (e.g., `/unleash` → `/uf.unleash`). Several commands also gained significant new capabilities: soft-gate CI causality analysis, structured PR descriptions, sub-agent conflict resolution, GitHub review posting, and multi-agent issue triage.

The website currently uses the old command names exclusively across 14+ content pages, blog posts, layout templates, and internal documentation. This design covers how to systematically update all references and document new capabilities.

## Goals / Non-Goals

### Goals
- Replace every old command name with its `uf.*` equivalent across all website content
- Document new command capabilities accurately (soft-gate, structured PRs, triage)
- Document new commands (`/uf.triage-issue`, `/forge`, `/org`, `/inbox`, `/handoff`)
- Preserve the persona name vs command name distinction (Cobalt-Crush as team member vs `/uf.cobalt-crush` as command)
- Ensure `npm run build` passes after all changes with no broken links or rendering issues
- Close all 7 tracked issues (#220, #221, #224, #179, #162, #204, #202) with the resulting PR

### Non-Goals
- Creating new documentation pages for commands that don't have existing coverage (e.g., a standalone `/uf.triage-issue` tutorial) — new commands are documented within existing pages only
- Restructuring the information architecture of the docs section
- Restructuring AGENTS.md governance prose or adding new governance sections — only updating stale command name references
- Changing blog post narratives or adding new blog posts — only updating command names in existing posts
- Updating openspec artifacts or spec files that reference old command names (these are historical records)

## Decisions

### D1: Bulk rename approach — find-and-replace with manual review

**Decision**: Use systematic grep + manual edit rather than automated sed replacement.

**Rationale**: Command names appear in different contexts — code blocks, prose, headings, link anchors, URL fragments, SCSS comments. A blind find-and-replace risks breaking markdown link syntax, URL anchors (e.g., `#autonomous-pipeline-unleash` → needs to become `#autonomous-pipeline-ufunleash`?), and inline code formatting. Each file will be reviewed in context to ensure the replacement is correct for that usage.

**Constitution alignment**: This supports Observable Quality (Principle III) — ensuring documentation accuracy through careful review rather than brittle automation.

### D2: URL anchor handling — preserve existing anchors

**Decision**: Keep existing URL fragment anchors unchanged (e.g., `#autonomous-pipeline-unleash` stays as-is). Only update the display text and code references.

**Rationale**: Changing heading text that generates anchors would break all internal cross-references and any external bookmarks. Hugo generates anchors from heading text — changing "Autonomous Pipeline (`/unleash`)" to "Autonomous Pipeline (`/uf.unleash`)" would change the anchor. Instead, we update the heading display but ensure any anchor-generating headings preserve their current slug or use explicit anchor IDs.

**Risk**: If Hugo auto-generates anchors from the new heading text, existing links break. Mitigation: Use Hugo's `{#anchor-id}` syntax or keep heading text identical where anchors are referenced, updating only code blocks and prose.

### D3: Persona name vs command name distinction

**Decision**: "Cobalt-Crush", "Divisor", and other persona names remain unchanged when used as team member references. Only the slash command syntax updates (e.g., "run `/cobalt-crush`" → "run `/uf.cobalt-crush`", but "the Cobalt-Crush developer agent" stays).

**Rationale**: Personas are identities, not commands. The team page for Cobalt-Crush describes the agent's role and philosophy — the persona name is part of the brand. The command `/uf.cobalt-crush` is how users invoke that persona.

### D4: New capability documentation — inline additions to existing pages

**Decision**: Document new capabilities (#221, #224, #179, #162, #204) by updating the relevant sections in existing pages rather than creating new standalone pages.

**Rationale**: The website already has comprehensive coverage of `/review-council`, `/review-pr`, and `/finale` in `common-workflows.md` and `code-review-tutorial.md`. Adding new capability details inline keeps related information together and avoids fragmenting the docs. The zero-waste mandate (AGENTS.md) also discourages creating pages without substantial standalone content.

### D5: New command documentation — add to common-workflows.md

**Decision**: Document `/uf.triage-issue`, `/forge`, `/org`, `/inbox`, and `/handoff` as new sections in `common-workflows.md`.

**Rationale**: `common-workflows.md` is the central command reference page. Adding new commands there maintains the single-source pattern. If any command grows complex enough to warrant its own tutorial, that can be a follow-up change.

### D6: Blog posts — minimal text changes only

**Decision**: Update command names in blog posts but do not rewrite narratives or add new capability descriptions.

**Rationale**: Blog posts are point-in-time artifacts. The narrative should remain coherent with the original publication context. Updating command names ensures readers can follow along with current tooling, but adding new capability descriptions would change the post's scope and potentially confuse the narrative arc. A future blog post can cover the namespace migration and new capabilities.

### D7: AGENTS.md — update command references

**Decision**: Update `/review-council` references in AGENTS.md to `/uf.review-council` since AGENTS.md is a living governance document, not a historical record.

**Rationale**: AGENTS.md instructs agents on current processes. Using stale command names would cause agents to invoke non-existent commands.

## Risks / Trade-offs

- **Risk**: Heading text changes may break Hugo-generated URL anchors. **Mitigation**: Audit all heading-based anchors referenced by internal links before changing heading text. Use explicit anchor syntax where needed.

- **Risk**: Blog post command name updates may create temporal inconsistency (post written when commands had old names, now shows new names). **Mitigation**: Accepted trade-off — readers following along need current command names. Posts do not include publication-date caveats for every command reference.

- **Risk**: Some pages have dense command references (common-workflows.md has 50+ references). Manual editing increases the chance of missed replacements. **Mitigation**: After all edits, run a final grep sweep for any remaining old command name patterns to catch stragglers.

- **Risk**: The code-review-tutorial.md page was recently written for `common-workflows.md` PR #74 and extensively references `/review-council` and `/review-pr`. Heavy edits may introduce formatting issues. **Mitigation**: Build verification after editing each major file.
