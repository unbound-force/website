## Context

The website documents convention packs, `uf init` language detection, and `uf doctor` health checks across two primary pages: the Developer guide (`content/docs/getting-started/developer.md`) and the CLI Reference (`content/docs/reference/cli.md`). Currently these pages reference Go and TypeScript as the only supported languages. Unbound Force v0.16.0 ships Python as a third supported language with a 46-rule convention pack, conditional doctor checks, and expanded language auto-detection.

The upstream implementation (unbound-force/unbound-force#254) has been merged and its design is documented in `openspec/changes/python-convention-pack/design`. This website change documents the feature for end users.

## Goals / Non-Goals

### Goals
- Document `python.md` and `python-custom.md` convention pack files in the Pack Files table
- Document Python marker files (`pyproject.toml`, `setup.py`, `setup.cfg`, `requirements.txt`, `tox.ini`, `Pipfile`) in the Language Auto-Detection section
- Document the conditional "Python Tools" doctor check group and its 9 check categories
- Update the `--lang` flag documentation to include `python` as a valid value
- Update file counts in the "What Gets Deployed" table to reflect added pack files
- Maintain consistency with the existing documentation style and structure

### Non-Goals
- Creating a standalone Python convention pack reference page (the full 46 rules are in the pack file itself; the website documents the pack's existence and purpose)
- Documenting the internal `isPythonProject()` or `detectLang()` implementation details
- Documenting the upstream design decisions (D1-D6) from the implementation spec
- Updating the homepage or navigation (no new pages are created)

## Decisions

### D1: Update existing pages rather than create new ones

All Python documentation fits naturally into the existing convention packs, language detection, and doctor sections of `developer.md` and `cli.md`. Creating separate pages would fragment the documentation and add maintenance burden. The existing page structure was designed to accommodate new languages -- the tables and lists simply need new rows.

### D2: Document tool-agnostic rule descriptions

The upstream Python convention pack uses outcome-focused rules rather than tool-specific rules (per upstream decision D3). The documentation should reflect this: describe what the pack checks for (formatting, linting, import sorting, security scanning) without mandating specific tools (ruff vs. black+flake8+isort). This matches how the Go and TypeScript pack descriptions work.

### D3: Document doctor checks as a conditional check group

The upstream implementation uses a separate "Python Tools" CheckGroup that is conditionally included when the project is detected as Python. The documentation should describe this as conditional behavior: "When a Python project is detected, `uf doctor` additionally checks for Python-specific tools." This matches the user's experience without exposing the internal `CheckGroup` abstraction.

### D4: List all 6 Python marker files in auto-detection

The upstream `detectLang()` expanded to check 6 Python markers: `pyproject.toml`, `setup.py`, `setup.cfg`, `requirements.txt`, `tox.ini`, `Pipfile`. The `--lang` flag description in `cli.md` currently lists auto-detection sources generically. The developer guide's Language Auto-Detection section should list all 6 markers explicitly, matching the specificity of the Go (`go.mod`) and TypeScript (`tsconfig.json`, `package.json`) entries.

## Risks / Trade-offs

### Risk: Documentation drift from future convention pack changes

The pack file table and doctor check descriptions are manual documentation of upstream code. If the upstream adds or removes Python rules or doctor checks, the website must be updated separately. This is the same risk that exists for Go and TypeScript documentation and is mitigated by the Website Documentation Gate process (filing issues when user-facing behavior changes).

### Risk: Version-specific content

The CLI reference page currently notes it reflects `uf v0.12.0`. The Python additions ship in v0.16.0. The version note should be updated to reflect the version these docs cover. If not updated, users on older versions may be confused by Python references.

### Trade-off: Concise doctor descriptions vs. full check listing

The developer guide describes doctor checks at a high level ("7 areas"). Adding full detail on all 9 Python check categories (python3, pip/uv, pytest, formatter, linter, import-sorter, security-scanner, mypy, tox) would make that section verbose. The design choice is to mention "Python-specific tool checks" in the high-level description and provide the full check list in a dedicated subsection or table, keeping the overview scannable while providing detail for those who need it.

### Risk: Adjacent pages referencing language support

Other pages reference supported languages but are out of scope for this change: the knowledge retrieval page has Go/TypeScript web source templates but no Python template, the architecture page lists only Go in its Tier 2 description, and the convention packs blog post's pack table omits Python. These are pre-existing gaps (TypeScript is also missing from some of these) and are tracked for separate follow-up rather than expanding this change's scope.
