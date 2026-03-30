# Quickstart: Project Documentation Update

**Branch**: `012-project-docs-update` | **Date**: 2026-03-30

## Verification Steps

After implementation, verify the documentation updates are correct and complete.

### Step 1: Build Verification

```bash
npm run build
```

The build must succeed with zero errors. This validates all Markdown frontmatter, internal links, and Hugo template rendering.

### Step 2: Gaze Page Verification (US1, US2, US3)

Open `content/docs/projects/gaze.md` and verify:

1. **Setup section**: `gaze init` description says "8 files" across "3 directories" (agents, commands, references) — NOT "four files"
2. **Three Modes section**: No mention of `/classify-docs` anywhere on the page
3. **Architecture table**: `aireport` package appears with a description of the AI CLI adapter
4. **Current Limitations**:
   - Assertion accuracy reads "~84.7%" (not 78.8%)
   - Single-package limitation is scoped to `gaze analyze` and `gaze quality` only
   - P3-P4 text clearly states P2 IS detected
5. **New content**: Fix strategy labels are documented (`add_tests`, `add_assertions`, `decompose`, `decompose_and_test`)

### Step 3: Tester Page Verification (US2, US3)

Open `content/docs/getting-started/tester.md` and verify:

1. **Core Commands**: `--ai=opencode` appears as an adapter option alongside `--ai=claude`
2. **CI Integration Pattern**: `--coverprofile` flag is documented with the two-step pattern (run tests separately, pass profile to gaze report)
3. **CI threshold**: `--max-crapload` example uses a realistic value (e.g., 50), not 15

### Step 4: Knowledge Page Verification (US4, US5)

Open `content/docs/getting-started/knowledge.md` and verify:

1. **Local Disk section**: Mentions automatic `.gitignore` respect — no configuration needed
2. **Local Disk section**: No longer claims "indexes all Markdown files including hidden directories" without qualification
3. **YAML example**: Includes `ignore` and `recursive` fields with syntax documentation
4. **New content**: `dewey doctor` is documented with its diagnostic sections
5. **New content**: `dewey reindex` is documented for clean index rebuilds
6. **New content**: Global CLI flags (`--verbose`, `--log-file`, `--no-embeddings`, `--vault`) are documented
7. **New content**: Troubleshooting section covers MCP timeout, Ollama not running, lock file conflicts, low embedding coverage

### Step 5: Common Workflows Verification (US6, US7, US8)

Open `content/docs/getting-started/common-workflows.md` and verify:

1. **New section**: `/unleash` is documented with the 8-step pipeline, exit points, Dewey-powered clarification, parallel Swarm workers, and resumability
2. **New section**: `/finale` is documented with the 9-step workflow, branch safety, CI check watching, and rebase-merge strategy
3. **Code Review section**: Phase 1a (CI hard gate) and Phase 1b (Gaze quality analysis) are documented
4. **Environment Setup**: `uf setup` step count reflects current behavior after the `opencode.json` move

### Step 6: Developer Page Verification (US7, US8)

Open `content/docs/getting-started/developer.md` and verify:

1. **Convention Packs table**: Lists 7 files (6 existing + `severity.md`)
2. **Severity pack**: Described as the shared calibration standard for CRITICAL/HIGH/MEDIUM/LOW findings
3. **`uf init` section**: Mentions `opencode.json` management (Dewey MCP entry, Swarm plugin entry)

### Step 7: Divisor Team Page Verification (US7)

Open `content/docs/team/the-divisor.md` and verify:

1. **Exclusive ownership**: Each review dimension is owned by exactly one persona (no duplicates)
2. **Severity standard**: References the shared severity pack
3. **Prior learnings**: Mentions Hivemind integration for context-aware reviews

### Step 8: Cross-Page Consistency

Search across all pages for these values and verify consistency:

| Claim                  | Expected Value      | Search Term               |
| ---------------------- | ------------------- | ------------------------- |
| Convention pack count  | 7                   | "pack" + count references |
| Assertion accuracy     | ~84.7%              | "84.7" or "assertion"     |
| `gaze init` file count | 8                   | "gaze init"               |
| `/classify-docs`       | Zero results        | "classify-docs"           |
| AI adapters            | opencode and claude | "--ai="                   |

### Step 9: Visual Verification

```bash
npm run dev
```

Open `http://localhost:1313/` and navigate to each modified page. Verify:

- Tables render correctly
- Code blocks have proper syntax highlighting
- Headings create correct table-of-contents entries
- Internal links work (click through to referenced pages)
- Dark mode renders correctly for all new content
