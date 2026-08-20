<!--
  [P] marks tasks eligible for parallel execution.
  Add [P] when a task: (a) touches different files from
  other [P] tasks in the group, (b) has no dependency
  on prior tasks in the group, (c) can safely execute
  without ordering constraints.
  Do NOT add [P] when tasks modify the same file —
  parallel workers will cause merge conflicts.
  Tasks without [P] run sequentially first, then [P]
  tasks run in parallel.
-->

## 1. Update Developer Guide (`content/docs/getting-started/developer.md`)

- [x] 1.1 Add `python.md` and `python-custom.md` rows to the Pack Files table (line ~204). `python.md` is tool-owned with description: "Python-specific rules: coding style, architectural patterns, security, testing, type annotations, documentation". `python-custom.md` is user-owned with description: "Project-specific Python conventions".
- [x] 1.2 Add Python marker files to the Language Auto-Detection section (line ~235). Add a bullet for Python detection: "`pyproject.toml`, `setup.py`, `setup.cfg`, `requirements.txt`, `tox.ini`, or `Pipfile` detected: deploys `python.md` + `python-custom.md`".
- [x] 1.3 Update the `--lang` usage example (line ~282) to include `python` as a valid language value: `uf init [--divisor] [--lang go|python|typescript] [--force]`.
- [x] 1.4 Update the `--lang` flag description in the Flags table (line ~290). Note: `pyproject.toml` is already listed as an auto-detection marker. Add the remaining 5 Python markers (`setup.py`, `setup.cfg`, `requirements.txt`, `tox.ini`, `Pipfile`) and explicitly mention Python as a detected language.
- [x] 1.5 Update the Convention Packs count in the "What Gets Deployed" table (line ~301) from 9 to 11, and add `python` to the examples list. Verify the current count is still 9 before updating — if upstream changes added other packs, adjust the target count accordingly.
- [x] 1.6 Update the doctor checks description (line ~34) to mention Python-specific tool checks as a conditional check area. Add a brief note that when a Python project is detected, doctor additionally checks for Python tooling (python3, pytest, formatters, linters, type checkers, etc.).
- [x] 1.7 Add a "Python Doctor Checks" subsection or note after the existing doctor description. List the 9 check categories with their status levels: python3 (required), pip/uv (recommended), pytest (required), formatter (recommended), linter (recommended), import sorter (recommended), security scanner (recommended), mypy (optional), tox (optional).

## 2. Update CLI Reference (`content/docs/reference/cli.md`)

- [x] 2.1 Update the `--lang` flag description (line ~42) to include Python markers in the auto-detection list. Change the description to list `python` as a valid value.
- [x] 2.2 Update the `uf doctor` description (line ~66) to mention that Python-specific tool checks are conditionally included when a Python project is detected.
- [x] 2.3 Update the version note (line ~11) from `v0.12.0` to `v0.16.0`.

## 3. Verification

- [x] 3.1 Run `npm run build` and verify no build errors.
- [x] 3.2 Run `npm run dev` and visually verify the updated sections render correctly on the Developer guide and CLI Reference pages in both light and dark mode.
- [x] 3.3 Verify all existing cross-reference links to the convention packs section still work (anchor IDs unchanged).
- [x] 3.4 Verify constitution alignment: this change is documentation-only with all principles assessed as N/A per the proposal.
