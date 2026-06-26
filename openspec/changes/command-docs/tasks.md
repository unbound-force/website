## 1. Add `/review-pr` to Code Review section

- [x] 1.1 Read upstream source file (`unbound-force/unbound-force/.opencode/command/review-pr.md`) via Dewey or direct access to verify current features before writing
- [x] 1.2 Add `/review-pr` documentation to the Code Review section of `content/docs/getting-started/common-workflows.md`: purpose, usage syntax (`/review-pr [N]`), auto-detection from current branch, `gh` CLI requirement
- [x] 1.3 Write CI causality analysis subsection: "PR-caused" vs "pre-existing" failure classification, fix branch offering for pre-existing failures (dirty-tree guard, user confirmation, collision handling)
- [x] 1.4 Document review capabilities: local deterministic tools (build, vet, lint), AI judgment (alignment, security, constitution), in-line PR comments (15-comment cap, human confirmation gate)
- [x] 1.5 Add comparison table: `/review-council` vs `/review-pr` — timing (pre-PR vs post-PR), scope, agent model (5+ parallel vs 1 agent), use cases, with a decision guide for choosing the right command

## 2. Add `/agent-brief` documentation

- [x] 2.1 Read upstream source file (`unbound-force/unbound-force/.opencode/command/agent-brief.md`) via Dewey or direct access to verify current features before writing
- [x] 2.2 Add "Project Context Management" section to `content/docs/getting-started/common-workflows.md` with `/agent-brief` overview
- [x] 2.3 Document create mode: generates AGENTS.md from project signals (go.mod/package.json, Makefile, CI config, README), hybrid template + LLM approach
- [x] 2.4 Document audit mode: 5-tier scoring rubric (Excellent/Strong/Adequate/Weak/Missing), deterministic structural checks, context-sensitive section detection (constitution, spec framework)
- [x] 2.5 Document bridge file management: CLAUDE.md and .cursorrules creation/validation — which files, when created vs validated, purpose
- [x] 2.6 Document doctor integration: `uf doctor` structural checks for AGENTS.md quality

## 3. Create quick reference table

- [x] 3.1 Create a quick reference table near the top of `common-workflows.md` listing all documented slash commands with brief descriptions: `/unleash`, `/finale`, `/speckit.specify`, `/opsx-propose`, `/cobalt-crush`, `/review-council`, `/review-pr`, `/agent-brief`

## 4. Verification

- [x] 4.1 Run `npm run build` and confirm no build errors
- [x] 4.2 Update `common-workflows.md` frontmatter `description` to reflect the expanded scope
- [ ] 4.3 Run `npm run dev` and verify all new sections render correctly
- [x] 4.4 Verify comparison table and quick reference table render correctly (no broken markdown table formatting)
- [ ] 4.5 Verify both light and dark mode rendering
<!-- spec-review: passed -->
<!-- code-review: passed -->
