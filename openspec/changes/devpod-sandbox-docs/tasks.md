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

## 1. New Getting Started Guides

- [x] 1.1 [P] Create `content/docs/getting-started/devenv-setup.md` — Development Environment Setup guide. Cover prerequisites (Podman, DevPod via `uf setup`), `uf sandbox init` devcontainer scaffolding, workspace lifecycle (create/start/stop/destroy), mount modes (isolated vs direct), IDE integration (`--ide` flag with resolution chain), gateway auto-start for cloud LLM providers, platform-specific notes for macOS (Podman machine, virtiofs UID mapping) and Linux (rootless containers, SELinux), troubleshooting (files as root:nobody, `uf doctor`). Cross-reference sandbox.md and cli.md for detailed flag syntax. Frontmatter: title "Development Environment Setup", description "Set up a DevPod-based containerized development environment for AI agent sessions", lead "Walk through DevPod sandbox setup from prerequisites to your first AI agent session." Set weight 25 (Getting Started section, after quick-start at 20, before role-specific guides starting at 30).

- [x] 1.2 [P] Create `content/docs/getting-started/multi-platform.md` — Multi-Platform Support guide. Cover `uf init --platform` flag (opencode, cursor), repeatable flag for dual-platform scaffolding, OpenCode-specific output (`.opencode/` directory, MCP config, convention packs), Cursor-specific output (`.cursor/` directory, `.mdc` rule files, `.cursor/mcp.json`), Divisor subset mode (`--divisor --platform cursor`), guidance on when to use each platform. Frontmatter: title "Multi-Platform Support", description "Scaffold projects for OpenCode, Cursor, or both AI coding platforms", lead "Configure uf init --platform to generate platform-specific configurations for your preferred AI coding environment." Set weight 27 (Getting Started section, after devenv-setup at 25, before role-specific guides starting at 30).

## 2. Reference Page Updates

- [x] 2.1 [P] Update `content/docs/reference/sandbox.md` — Mark DevPod as primary CDE backend, label Eclipse Che/Dev Spaces as legacy/deprecated with migration note, document `--ide` flag resolution chain (CLI flag > `UF_SANDBOX_IDE` env var > `.uf/config.yaml` `sandbox.ide` > default `none`), document `--provider podman` flag behavior (new flag from issue #199, references registered DevPod provider name, not PATH lookup), add diagnostic hint on `devpod up` failure pointing to `uf doctor`/`uf setup`, add cross-reference to `uf ollama-proxy` for GPU-less workflows, add cross-reference to the Development Environment Setup guide in the See Also section. Note: existing sandbox.md already partially covers IDE selection and DevPod — focus on updating labels, adding the `--provider` flag, and strengthening the deprecation notice rather than duplicating existing content.

- [x] 2.2 [P] Update `content/docs/reference/cli.md` — Add `uf ollama-proxy` command section with subcommands (start, stop, status) and configuration (port, embed model, gateway URL). Update `uf sandbox` subcommands to include `--ide` and `--provider` flags. Update `uf setup` description to note Podman and DevPod installation steps (14-16). Update `uf doctor` description to note Podman and DevPod health checks with severity levels (Podman=Fail, DevPod=Warn). Add cross-reference to Multi-Platform Support guide from the `uf init` section.

## 3. Changelog Update

- [x] 3.1 Update `content/docs/changelog/_index.md` — Add release entries for: DevPod as primary CDE backend (Che deprecated), `--ide` flag for sandbox IDE selection, `uf ollama-proxy` command, `uf init --platform` for multi-platform scaffolding, DevPod provider pre-flight simplification, Podman/DevPod added to `uf setup` and `uf doctor`.

## 4. Verification

- [x] 4.1 Run `npm run build` — Confirm build succeeds with no errors. Verify no broken links or missing pages.

- [x] 4.2 Constitution alignment verification — Confirm all changes are static Markdown content only. No agent artifacts, output formats, composability, testability, or security properties are affected. All constitution principles assessed as N/A per proposal.md.

<!-- spec-review: passed -->
