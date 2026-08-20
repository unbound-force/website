## ADDED Requirements

### Requirement: Python Convention Pack Documentation

The Developer guide Pack Files table MUST include entries for `python.md` (tool-owned) and `python-custom.md` (user-owned) with descriptions consistent in style with the existing Go and TypeScript entries.

The `python.md` description MUST mention the 7 rule sections: Coding Style, Architectural Patterns, Security Checks, Testing Conventions, Type Annotations, Documentation Requirements, and Custom Rules.

#### Scenario: User reads convention pack documentation
- **GIVEN** a user visits the Developer guide convention packs section
- **WHEN** the user views the Pack Files table
- **THEN** `python.md` and `python-custom.md` appear in the table with ownership and purpose descriptions

### Requirement: Python Language Auto-Detection Documentation

The Developer guide Language Auto-Detection section MUST list all 6 Python marker files used by `detectLang()`: `pyproject.toml`, `setup.py`, `setup.cfg`, `requirements.txt`, `tox.ini`, `Pipfile`.

The documentation MUST indicate that detecting any of these markers triggers deployment of `python.md` + `python-custom.md`.

#### Scenario: User checks which files trigger Python detection
- **GIVEN** a user has a Python project with only a `requirements.txt` file
- **WHEN** the user reads the Language Auto-Detection section
- **THEN** the user sees that `requirements.txt` is listed as a Python marker file that triggers Python convention pack deployment

### Requirement: Python Doctor Checks Documentation

The Developer guide MUST document that `uf doctor` runs Python-specific tool checks when a Python project is detected. The documentation MUST list the check categories and their status levels (required, recommended, optional).

The 9 check categories are:
- `python3` (required)
- `pip` or `uv` (recommended, either satisfies)
- `pytest` (required)
- Code formatter (recommended, checks for alternatives)
- Linter (recommended, checks for alternatives)
- Import sorter (recommended, checks for alternatives)
- Security scanner (recommended, checks for alternatives)
- `mypy` (optional)
- `tox` (optional)

#### Scenario: User runs uf doctor on a Python project
- **GIVEN** a user reads the doctor documentation
- **WHEN** the user has a Python project (detected via marker files)
- **THEN** the documentation explains that a "Python Tools" check group runs with 9 categories covering required, recommended, and optional tools

### Requirement: CLI Reference Python Updates

The CLI Reference `--lang` flag description MUST list `python` as a valid language value alongside `go` and `typescript`.

The CLI Reference `uf doctor` description SHOULD mention that Python-specific checks are included when a Python project is detected.

#### Scenario: User checks --lang flag options
- **GIVEN** a user visits the CLI Reference page
- **WHEN** the user reads the `--lang` flag description
- **THEN** `python` appears as one of the valid language options

## MODIFIED Requirements

### Requirement: Convention Pack Count in "What Gets Deployed" Table

Previously: "Convention Packs | 9 | default, go, typescript, content (each with tool-owned and user-owned variants) + severity"

The convention pack count MUST be updated from 9 to 11. The examples list MUST include `python` alongside `go`, `typescript`, and `content`.

#### Scenario: User checks what uf init deploys
- **GIVEN** a user reads the "What Gets Deployed" table
- **WHEN** the user looks at the Convention Packs row
- **THEN** the count shows 11 and the examples include default, go, python, typescript, and content

### Requirement: Doctor Check Areas Count

Previously: "Doctor checks 7 areas: your detected environment (version managers), core tools, Replicator health, scaffolded files, hero availability, MCP server config, and agent/skill integrity."

The description MUST be updated to reflect that Python-specific tool checks are conditionally included as an additional check area when a Python project is detected.

#### Scenario: User reads doctor overview
- **GIVEN** a user reads the doctor description in the Developer guide
- **WHEN** the user has a Python project
- **THEN** the documentation indicates that doctor checks additional areas for Python tool availability

### Requirement: CLI Reference Version Note

Previously: "This page reflects `uf` v0.12.0."

The version note MUST be updated to reflect the current version that includes Python support.

## REMOVED Requirements

None.
