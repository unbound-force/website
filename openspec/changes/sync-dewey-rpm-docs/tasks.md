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

## 1. Add RPM Installation Sections

- [x] 1.1 [P] Add RPM install section to `content/docs/projects/dewey.md` between the Homebrew cask block and the source install section. Insert after the Ollama code block (line 28) and before "On Linux, install from source:" (line 30). Also update "On Linux, install from source:" to "Or install from source:" since RPM is also a Linux method. Include: download `.rpm` from GitHub Releases link, `sudo dnf install` command, note `amd64` and `arm64` availability.
- [x] 1.2 [P] Add RPM install section to `content/docs/getting-started/knowledge.md` immediately after the Homebrew code block (after line 35), before the embedding model explanation paragraph (line 37). This keeps all installation methods grouped together before the configuration subsections. Include: download `.rpm` from GitHub Releases link, `sudo dnf install` command, note `amd64`/`arm64` availability, binary installs to `/usr/bin/dewey`.

## 2. Verification

- [x] 2.1 Run `npm run build` and verify no build errors
- [x] 2.2 Run `npm run dev` and visually verify both pages render the RPM section correctly between Homebrew and source install
- [x] 2.3 Verify content accuracy against upstream Dewey README (dewey#59) — commands, URL patterns, and install paths must match
<!-- spec-review: passed -->
<!-- code-review: passed -->
