## 1. Add `/review-pr` to Code Review section

- [ ] 1.1 Add `/review-pr` documentation to the Code Review section of `content/docs/getting-started/common-workflows.md`: purpose, usage syntax (`/review-pr [N]`), auto-detection from current branch, `gh` CLI requirement
- [ ] 1.2 Write CI causality analysis subsection: "PR-caused" vs "pre-existing" failure classification, fix branch offering for pre-existing failures (dirty-tree guard, user confirmation, collision handling)
- [ ] 1.3 Document review capabilities: local deterministic tools (build, vet, lint), AI judgment (alignment, security, constitution), in-line PR comments (15-comment cap, human confirmation gate)
- [ ] 1.4 Add comparison table: `/review-council` vs `/review-pr` — timing (pre-PR vs post-PR), scope, agent model (5+ parallel vs 1 agent), use cases

## 2. Add `/agent-brief` documentation

- [ ] 2.1 Add `/agent-brief` section to `content/docs/getting-started/common-workflows.md` (or create a separate page if content exceeds ~200 lines)
- [ ] 2.2 Document create mode: generates AGENTS.md from project signals (go.mod/package.json, Makefile, CI config, README), hybrid template + LLM approach
- [ ] 2.3 Document audit mode: 5-tier scoring rubric (Excellent/Strong/Adequate/Weak/Missing), 12 structural checks, context-sensitive section detection (constitution, spec framework)
- [ ] 2.4 Document bridge file management: CLAUDE.md and .cursorrules creation/validation
- [ ] 2.5 Document doctor integration: 12 deterministic structural checks

## 3. Update quick reference table

- [ ] 3.1 Add `/review-pr [N]` entry to the quick reference table in `common-workflows.md` with brief description
- [ ] 3.2 Add `/agent-brief` entry to the quick reference table with brief description

## 4. Verification

- [ ] 4.1 Run `npm run build` and confirm no build errors
- [ ] 4.2 Run `npm run dev` and verify all new sections render correctly in common-workflows.md
- [ ] 4.3 Verify both light and dark mode rendering
- [ ] 4.4 Verify comparison table renders correctly (no broken markdown table formatting)
