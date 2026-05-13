## 1. Create blog post file with frontmatter

- [x] 1.1 Create `content/blog/agents-md-quality.md` with Hugo/Doks blog frontmatter (title, description, lead, slug, date, draft: false, weight, toc, categories: ["Engineering"], tags: ["agents-md", "context", "quality", "agent-brief"], contributors)

## 2. Write the problem and solution sections

- [x] 2.1 Write "The Problem" section: AGENTS.md as the prompt shaping all agent output — weak context leads to code that compiles but does not fit. Frame universally (Claude Code, Cursor, Copilot, OpenCode)
- [x] 2.2 Read upstream source file (`unbound-force/unbound-force/.opencode/command/agent-brief.md`) to verify section taxonomy, rubric tiers, and structural checks before writing
- [x] 2.3 Write section taxonomy: Tier 1 (essential: Project Overview, Build & Test, Project Structure, Code Conventions, Technology Stack), Tier 1C (context-sensitive: Constitution, Spec Framework), Tier 2 (advanced: Architecture, Testing, Git & Workflow, Behavioral Constraints)

## 3. Write rubric and audit sections

- [x] 3.1 Write 5-tier scoring rubric table: Excellent, Strong, Adequate, Weak, Missing — with criteria for each tier
- [x] 3.2 Write `/agent-brief audit` section: 12 structural checks, actionable recommendations, context-sensitive detection
- [x] 3.3 Write `/agent-brief create` section: project signal analysis (go.mod, Makefile, CI config, README), hybrid template + LLM approach

## 4. Write supplementary sections

- [x] 4.1 Write bridge files section: CLAUDE.md (@AGENTS.md import), .cursorrules, cross-tool compatibility
- [x] 4.2 Write call to action: install command verified at implementation time

## 5. Cross-references and verification

- [x] 5.1 Add cross-reference links to common-workflows docs and getting-started pages — only link to pages that exist at implementation time
- [x] 5.2 Run `npm run build` and confirm no build errors
- [x] 5.3 Verify the blog post appears in the blog listing page
- [x] 5.4 Verify all internal links resolve (no dead links)
- [ ] 5.5 Run `npm run dev` and verify the post renders correctly
- [ ] 5.6 Verify both light and dark mode rendering
<!-- spec-review: passed -->
<!-- code-review: passed -->
