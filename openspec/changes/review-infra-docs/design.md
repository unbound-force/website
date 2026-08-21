## Context

The Unbound Force website documents the organization's AI agent swarm for software engineering. Ten GitHub issues describe shipped features and capabilities that have no corresponding website documentation: convention packs, updated branch naming, review infrastructure commands, the council-review-action GitHub Action, Constitution Principle V, and internal toolchain improvements.

The proposal identifies three categories of work: new reference pages, new tutorial content, and updates to existing pages. All changes are Markdown content files within the existing Hugo/Doks site structure. No templates, SCSS, configuration, or build infrastructure changes are needed.

## Goals / Non-Goals

### Goals

- Document convention packs as a concept with a dedicated reference page, covering structure (MUST/SHOULD/MAY severity), loading mechanism, and the CI pack as an example
- Add council-review-action reference documentation and a step-by-step adoption tutorial
- Add CLI reference entries for `/agent-brief`, `/address-feedback`, and `/review-pr`
- Add Constitution Principle V (Security by Default) to the constitution page
- Correct branch naming from `NNN-<name>` to `speckit/NNN-<name>` in all content pages containing the old pattern (common-workflows.md, developer.md)
- Cross-reference new content from existing pages (developer guide, constitution governance hierarchy)

### Non-Goals

- Documenting every convention pack rule verbatim (the reference page describes the system; individual packs live in source repos)
- Rewriting the code-review-tutorial.md (it already covers `/review-pr` at the tutorial level)
- Documenting review-context skill internals or replicator scaffold internals (these are contributor-facing details that belong in AGENTS.md or upstream repos, not the public website)
- Adding navigation menu entries (new pages appear in the sidebar automatically via Hugo section hierarchy)

## Decisions

### D1: Convention packs get a reference page, not a getting-started page

Convention packs are a lookup resource (what are they, what packs exist, what severity levels mean) rather than a sequential learning topic. Placing the page at `content/docs/reference/convention-packs.md` follows the existing pattern where reference/ contains tool and concept documentation (cli.md, config.md, gateway.md, sandbox.md). The developer guide will cross-reference this page for workflow context.

### D2: CLI commands are added to the existing cli.md rather than separate pages

The existing cli.md documents `uf` CLI subcommands. The three new commands (`/agent-brief`, `/address-feedback`, `/review-pr`) are OpenCode slash commands, not `uf` subcommands — they have a different invocation model (typed in an AI coding session, not a terminal). Adding them to cli.md under a clearly delineated new section ("OpenCode Slash Commands") with an introductory paragraph explaining the difference avoids fragmenting the reference across multiple pages while maintaining accuracy about command types.

### D3: Council-review-action gets both reference and tutorial pages

The council-review-action is a standalone GitHub Action that downstream repos adopt independently. It needs both a reference page (configuration options, workflow chain, persona discovery) and a tutorial (step-by-step adoption guide). This follows the existing pattern where code-review-tutorial.md complements the CLI reference.

### D4: Principle V is added inline to the existing constitution page

The constitution page documents all principles in a single page with a consistent structure (heading, description, MUST rules, SHOULD rules, rationale). Principle V follows the same pattern. This keeps the constitution as a single authoritative document rather than splitting it across pages.

### D5: Review-context skill and replicator scaffold updates are handled via cross-references

Issues #226 (review-context skill) and #231 (replicator scaffold) describe internal toolchain improvements. Rather than creating dedicated website pages for internal mechanics, relevant user-facing impacts will be noted in the developer guide and convention pack reference. Contributors needing deeper detail are directed to upstream AGENTS.md files.

### D6: Branch naming is a corrective update, not a new page

The `speckit/NNN-<name>` branch prefix (#205) replaces the old `NNN-<name>` convention. This is a find-and-replace across all content pages containing the old pattern (common-workflows.md and developer.md), not a documentation expansion. AGENTS.md also contains the old pattern but is out of scope for this change. The change is small but important for accuracy.

## Risks / Trade-offs

### Risk: Upstream content drift

Convention pack rules and council-review-action configuration may change in upstream repos after this documentation is published. **Mitigation**: Document the concept and structure rather than exhaustively listing every rule. Link to source repositories for canonical rule definitions. This follows the Content Accuracy constraint in AGENTS.md — describe what exists without overstating.

### Risk: Principle V sourcing accuracy

Constitution Principle V content must be sourced from the actual upstream constitution, not fabricated. **Mitigation**: Implementation tasks must read the upstream `.specify/memory/constitution.md` or the relevant GitHub issue (#142) to extract the exact principle text, MUST rules, and rationale. The website page adapts (not copies) the upstream content for a website audience.

### Trade-off: Internal toolchain docs excluded

Issues #226 and #231 describe contributor-facing toolchain improvements that are not documented on the public website. This reduces the issue coverage from 10/10 to 8/10 in terms of dedicated pages. **Accepted** because the website audience is users and adopters, not internal swarm contributors. Internal toolchain docs belong in upstream AGENTS.md files.
