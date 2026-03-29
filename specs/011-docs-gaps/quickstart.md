# Quickstart: Documentation Gaps

## Overview

Create 2 new pages and expand 2 existing pages to address the 5 documentation gaps identified in the audit.

## Implementation Order

Work through pages in dependency order -- pages that other pages link to should be created first.

### Phase 1: New Pages (P1)

These pages are referenced by existing pages and each other.

1. **`constitution.md`** -- New page. Source: org constitution v1.1.0. Content: 4 principles with MUST/SHOULD rules, governance model, hero alignment, Constitution Check process. Cross-link to artifacts page.

2. **`artifacts.md`** -- New page. Source: AGENTS.md artifact types. Content: envelope format, 7 artifact types with producer/consumer mappings, lifecycle stage mapping. Cross-link to common-workflows (lifecycle) and constitution (Principle I).

### Phase 2: Workflow Commands (P2)

3. **`common-workflows.md`** -- Add Workflow Management section after existing Workflow Commands/Configuration sections. Content: `/workflow status`, `/workflow list`, `/workflow advance` with usage syntax and behavior descriptions.

### Phase 3: Developer Guide Expansions (P2+P3)

4. **`developer.md`** -- Expand convention packs section (6 pack files, ownership model, customization, language detection). Add `uf init` standalone section (flags, file categories, ownership model).

## Verification

After all changes:

```bash
npm run build          # Must succeed with zero errors
npm run dev            # Visual check: all pages render, sidebar navigation correct
```

Cross-page checks:

- Constitution page links to artifacts page for envelope format details
- Artifacts page links to common-workflows for lifecycle stages
- All new pages appear in sidebar navigation within two clicks of homepage
- No duplicate content between new pages and existing pages
