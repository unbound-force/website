# Quickstart: v0.5.0 Documentation Sync

## Overview

Update 8 Markdown files and verify consistency across the website to reflect v0.5.0 upstream changes.

## Implementation Order

Work through files in dependency order -- pages that other pages link to should be updated first.

### Phase 1: Setup and Workflow Docs (P1)

These pages are the foundation -- other pages reference them.

1. **`common-workflows.md`** -- The most changes. Update:
   - Environment Setup section (uf setup steps, uf init behavior, Dewey now included)
   - Add `/workflow seed` and `/workflow start` flag documentation
   - Add `.unbound-force/config.yaml` documentation
   - Add `opsx/` branch convention to OpenSpec section
   - Add embedding alignment env vars note

2. **`developer.md`** -- Update uf setup description and add branch convention note.

3. **`product-owner.md`** -- Add `/workflow seed` command reference.

### Phase 2: Team and Knowledge Pages (P2)

4. **`team/the-divisor.md`** -- Rewrite council section (3 → 5 personas).

5. **`team/_index.md`** -- Fix "three" → "five" in Divisor description.

6. **`knowledge.md`** -- Add embedding model alignment section.

### Phase 3: Consistency Fixes (P3)

7. **`team/dewey.md`** -- Fix MCP tool count.

8. **`projects/dewey.md`** -- Fix MCP tool count.

## Verification

After all changes:

```bash
npm run build          # Must succeed with zero errors
npm run dev            # Visual check: all updated pages render correctly
```

Cross-page consistency checks:

- Search for "sub-persona" / "persona" counts -- all must say "five"
- Search for MCP tool counts -- all must say "40 tools across 10 categories"
- Search for "uf setup" descriptions -- all must reflect v0.5.0 behavior
- Verify all internal links still resolve
