## Why

Unbound Force v0.16.0 adds first-class Python support (unbound-force/unbound-force#254): a `python.md` convention pack with 46 rules across 7 sections, Python-specific `uf doctor` health checks, and Python project detection in `uf init`. The website documentation currently references only Go and TypeScript as supported languages. Without updating the docs, Python users will not know that convention packs, doctor checks, and auto-detection exist for their projects.

GitHub issue: unbound-force/website#201 (milestone: release v0.16.0).

## What Changes

- **Convention Packs section** (`developer.md`): Add `python.md` and `python-custom.md` to the pack files table. Add Python marker files to the language auto-detection list. Update the "What Gets Deployed" table to reflect the new convention pack count.
- **CLI Reference** (`cli.md`): Update the `--lang` flag description to list Python alongside Go and TypeScript. Update the `uf doctor` description to mention Python-specific tool checks when a Python project is detected.
- **Developer guide doctor section**: Update the doctor check areas count and description to mention Python tool checks (python3, pip/uv, pytest, formatter, linter, mypy, etc.) as a conditional check group.

## Capabilities

### New Capabilities
- `Python convention pack documentation`: Reference documentation for the 46-rule `python.md` pack covering Coding Style, Architectural Patterns, Security Checks, Testing Conventions, Type Annotations, Documentation Requirements, and Custom Rules
- `Python doctor checks documentation`: Documentation of the conditional "Python Tools" check group that activates when `isPythonProject()` returns true
- `Python language auto-detection documentation`: Documentation of the 6 Python marker files (`pyproject.toml`, `setup.py`, `setup.cfg`, `requirements.txt`, `tox.ini`, `Pipfile`) used by `detectLang()`

### Modified Capabilities
- `Convention pack file table`: Expanded to include `python.md` and `python-custom.md` entries
- `Language auto-detection list`: Updated to include Python marker files
- `uf init --lang flag`: Updated to list `python` as a valid language option
- `uf init "What Gets Deployed" table`: Convention pack count updated from 9 to 11

### Removed Capabilities
- None

## Impact

- **Files modified**: `content/docs/getting-started/developer.md`, `content/docs/reference/cli.md`
- **No new files**: All changes are updates to existing documentation pages
- **No layout/navigation changes**: Content additions within existing sections
- **Cross-references**: Existing links to convention packs sections remain valid (anchor IDs unchanged)

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

This change modifies documentation content only. No agent artifacts, inter-agent communication, or artifact formats are affected.

### II. Composability First

**Assessment**: N/A

Documentation changes do not affect hero composability or standalone functionality. The Python convention pack itself (documented here) follows the same composable pack architecture as Go and TypeScript packs.

### III. Observable Quality

**Assessment**: N/A

No machine-parseable output or provenance metadata is affected. The documentation describes existing observable outputs (doctor check results, convention pack rule IDs).

### IV. Testability

**Assessment**: N/A

Documentation-only change. Validation is `npm run build` succeeding and visual verification of rendered pages.

### V. Security by Default

**Assessment**: N/A

No dependencies added, no inputs processed, no permissions changed. Documentation content does not affect supply chain integrity or security posture.
