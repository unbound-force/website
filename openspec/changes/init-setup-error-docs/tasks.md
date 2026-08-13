## 1. CLI Reference Updates

- [x] 1.1 [P] Update `content/docs/reference/cli.md`: Add a sentence to the `init` section (after the flags table, before `## setup`) noting that if any sub-tool fails during initialization, the actual error output from the failing command is displayed
- [x] 1.2 [P] Update `content/docs/reference/cli.md`: Add a sentence to the `setup` section (after the flags table, before the "See the Quick Start" link) noting that if a tool installation or configuration step fails, the actual error output is displayed

## 2. Developer Guide Update

- [x] 2.1 Update `content/docs/getting-started/developer.md`: Add a note at the end of the "Sub-Tool Initialization" section (after the bullet list of sub-tool steps) stating that if any initialization step fails, the actual error output from the failing tool is displayed to help diagnose the issue

## 3. Verification

- [x] 3.1 Run `npm run build` and confirm no build errors
- [x] 3.2 Update the CLI reference version note from `v0.12.0` to reflect the version that includes this change (verify against upstream release; if the release version is not yet known, leave `v0.12.0` and note the pending update in the PR description)

<!-- spec-review: passed -->
