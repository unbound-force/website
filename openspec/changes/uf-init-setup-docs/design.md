## Context

The website currently documents `uf setup` and `uf init` in fragmented mentions across Quick Start, Developer Guide, and Common Workflows. Users can figure out how to run the commands but lack understanding of what happens inside -- the 12-step init process, sub-tool concurrency, `--force` re-initialization groups, guardrail injection, and troubleshooting.

Eight resolved GitHub issues (#203, #210, #211, #222, #223, #225, #236, #245) contain operational knowledge about these commands that exists only in issue/PR descriptions. The proposal calls for a dedicated lifecycle section on the Common Workflows page, plus cross-references from Quick Start and Developer Guide.

Per the proposal's constitution alignment: this change is documentation-only (N/A for Autonomous Collaboration, Observable Quality, Testability; PASS for Composability First since it documents the composable nature of sub-tool initialization).

## Goals / Non-Goals

### Goals
- Add a comprehensive `uf init` / `uf setup` lifecycle section to Common Workflows that explains what each command does, how sub-tool initialization works, `--force` behavior, and common issues.
- Add cross-references from Quick Start and Developer Guide to the new section.
- Source all technical details from resolved GitHub issues and existing documentation -- no fabrication.
- Maintain the existing page structure and navigation (no new pages, no menu changes).

### Non-Goals
- CLI reference documentation (flags, exit codes, full option listing) -- that belongs in the reference section, not getting-started.
- Documenting the internal Go implementation of `uf init` or `uf setup`.
- Documenting `uf doctor` in more depth (already covered adequately in Quick Start and Developer Guide).
- Creating a separate dedicated page -- the lifecycle section fits naturally within Common Workflows alongside the existing Environment Setup section.

## Decisions

### D1: Add section to Common Workflows rather than creating a new page

**Rationale**: Common Workflows already has an "Environment Setup" section (lines 396-465) that covers Install CLI, Run Setup, Verify, and Start Working. The lifecycle section extends this existing content with deeper operational detail. A separate page would fragment the setup documentation further when the goal is consolidation. The Common Workflows page already serves as the "how things work end-to-end" reference.

**Placement**: Insert the new lifecycle section immediately after the existing "Environment Setup" section and before "Next Steps". This keeps the learning progression: quick setup steps first, then detailed lifecycle for users who want to understand more.

### D2: Organize by command (`uf setup` then `uf init`) rather than by feature

**Rationale**: Users encounter `uf setup` first (it calls `uf init` as its final step). Organizing by command matches the user's mental model: "I ran setup, then init happened -- what did each do?" Feature-based organization (e.g., "guardrails", "sub-tools") would require users to already know the architecture.

### D3: Include a troubleshooting subsection within the lifecycle section

**Rationale**: The three most common issues (Dewey hang on `--force`, stale command references, RPM version resolution on Fedora) are directly caused by lifecycle behavior. Placing troubleshooting adjacent to the lifecycle explanation connects the problem to the cause. A separate troubleshooting page would lose this context.

### D4: Cross-references use inline links, not callout boxes

**Rationale**: Quick Start and Developer Guide already have dense content. A callout box ("Want to learn more about setup? See...") would add visual weight. An inline link within existing prose is less disruptive and follows the pattern already used on these pages.

## Risks / Trade-offs

### Common Workflows page length
Adding ~150-200 lines to an already 465-line page. **Mitigation**: The page has `toc: true`, so the new section appears in the table of contents for direct navigation. The Doks theme handles long pages well with its sticky TOC sidebar.

### Content sourced from issues may go stale
Issue details reflect the state at PR merge time. Future changes to `uf init`/`uf setup` could make the documentation inaccurate. **Mitigation**: The documentation describes behavior at a conceptual level (e.g., "sub-tools initialize concurrently") rather than implementation details (e.g., "goroutines with errgroup"). This makes it more resilient to internal refactoring. The website documentation gate (AGENTS.md) requires a website issue when user-facing CLI behavior changes.

### Fedora/RHEL details may not apply to most users
`uf setup` RPM version resolution (#236) is platform-specific. **Mitigation**: Platform-specific details are in a clearly labeled subsection so macOS/Homebrew users can skip them.

### Overlap with `init-setup-error-docs` OpenSpec change
The sibling OpenSpec change `init-setup-error-docs` also modifies `content/docs/getting-started/developer.md`, adding error output notes to the "Sub-Tool Initialization" section. This change (task 2.2) adds a cross-reference link in the Prerequisites section, which is a different section of the same file. The two changes are complementary -- the lifecycle section in Common Workflows provides operational context that complements the error output notes in the Developer Guide. No content duplication or merge conflict is expected, but implementers should verify line numbers if the sibling change lands first.
