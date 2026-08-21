# Proposal: Review Infrastructure Documentation

## Why

The Unbound Force website currently has significant documentation gaps around review infrastructure, convention packs, and recently-shipped capabilities. Ten GitHub issues (#206, #205, #226, #231, #190, #192, #142, #60, #59, #138) describe features and changes that exist in the codebase but have no corresponding website documentation. Users cannot discover these capabilities, and contributors cannot understand the review lifecycle end-to-end.

Specific gaps:

- **Convention packs** are referenced in existing docs (constitution page, governance hierarchy) but have no dedicated documentation. The CI convention pack (#206) adds 12 rules for CI workflow authoring that are entirely undocumented.
- **Branch naming** changed from `NNN-<name>` to `speckit/NNN-<name>` (#205), but existing docs (common-workflows.md) still reference the old pattern.
- **Three CLI commands** (`/agent-brief`, `/review-pr`, `/address-feedback`) have no reference documentation. `/review-pr` has tutorial coverage in code-review-tutorial.md but no CLI reference entry.
- **Council-review-action** (#190, #192) is a composite GitHub Action for AI code review with no docs or adoption tutorial.
- **Constitution Principle V: Security by Default** (#142) was added upstream but the website constitution page still only documents Principles I-IV.
- **Review-context skill** (#226) and **replicator scaffold updates** (#231) represent internal toolchain improvements that need documentation for contributors.

Without these docs, the website presents an incomplete and outdated picture of the system. Users hitting convention pack violations, CI review failures, or branch naming errors have nowhere to look for guidance.

## What Changes

### New Pages

1. **Convention Packs reference page** (`content/docs/reference/convention-packs.md`) — explains what convention packs are, how they're structured (MUST/SHOULD/MAY severity levels), how they're loaded, and links to the available packs. Covers issue #206 (CI pack) as one of the documented packs.

2. **Council Review Action reference page** (`content/docs/reference/council-review-action.md`) — documents the composite GitHub Action: three-workflow chain, persona discovery, configuration options, and integration with the review lifecycle. Covers issue #190.

3. **Council Review Action tutorial** (`content/docs/getting-started/council-review-action-tutorial.md`) — step-by-step adoption guide for adding the council-review-action to a downstream repository. Covers issue #192.

4. **Agent Brief command reference** (addition to `content/docs/reference/cli.md`) — documents `/agent-brief` create and audit modes, AGENTS.md lifecycle management. Covers issue #60.

5. **Address Feedback command reference** (addition to `content/docs/reference/cli.md`) — documents `/address-feedback` four-phase workflow for PR review feedback triage. Covers issue #138.

6. **Review PR command reference** (addition to `content/docs/reference/cli.md`) — documents `/review-pr` post-PR GitHub review, CI causality analysis. Covers issue #59.

### Modified Pages

7. **Constitution page** (`content/docs/getting-started/constitution.md`) — add Principle V: Security by Default, covering supply chain integrity, input validation, least privilege, and dependency justification as defined in the upstream constitution. Covers issue #142.

8. **Common Workflows page** (`content/docs/getting-started/common-workflows.md`) and **Developer guide** (`content/docs/getting-started/developer.md`) — update branch naming from `NNN-<name>` to `speckit/NNN-<name>` in all pages containing the old pattern. Covers issue #205.

9. **Developer guide** (`content/docs/getting-started/developer.md`) — add or expand convention pack section to cross-reference the new reference page and explain CI pack integration. Partial coverage of issues #206, #226, #231.

## Capabilities

### New

- Convention pack reference documentation (what they are, how they work, available packs)
- Council-review-action reference and tutorial pages
- `/agent-brief`, `/address-feedback`, `/review-pr` CLI reference entries
- Constitution Principle V documentation

### Modified

- Constitution page expanded from 4 to 5 principles
- Common workflows updated with current branch naming convention
- Developer guide expanded with convention pack cross-references

### Removed

None.

## Impact

- **Navigation**: Two new pages in the Reference section (convention-packs, council-review-action). One new page in Getting Started (council-review-action tutorial). CLI reference page gains three new command sections. No menu changes needed — these pages appear in the sidebar automatically via Hugo section hierarchy.
- **Existing content**: Constitution page gains a new section (additive). Common workflows page gets a find-and-replace on branch naming patterns (corrective). Code review tutorial is not modified — it already covers `/review-pr` at the tutorial level.
- **Build**: All changes are Markdown content files. No template, SCSS, or configuration changes required. `npm run build` validates all pages render.
- **Cross-references**: New convention pack page will be linked from the constitution governance hierarchy section and the developer guide. Council-review-action pages cross-reference the code review tutorial.

## Constitution Alignment

### Principle I: Autonomous Collaboration — PASS

This change documents artifact-based communication patterns (convention packs, review artifacts, council review output). All new documentation describes how heroes exchange work through files and schemas, reinforcing Principle I.

### Principle II: Composability First — PASS

Convention pack documentation explicitly covers how packs are independently loadable and composable. Council-review-action docs show how it integrates without requiring the full swarm. This directly supports composability.

### Principle III: Observable Quality — PASS

Documenting convention pack severity levels (MUST/SHOULD/MAY), CI convention rules, and review infrastructure improves transparency of quality enforcement. Users can now understand how quality claims are automated and verified.

### Principle IV: Testability — N/A

This change is documentation-only. No code is produced, so testability constraints do not apply. Validation is via `npm run build` and visual inspection per AGENTS.md.

### Principle V: Security by Default — N/A

This change is documentation-only. No code, dependencies, or secrets are introduced. The documentation of Principle V itself is sourced from the upstream constitution to ensure accuracy.
