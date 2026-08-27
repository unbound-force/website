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

## 1. Update sandbox reference prerequisites

- [x] 1.1 [P] Update the Prerequisites paragraph in `content/docs/reference/sandbox.md` (line 15): split into ephemeral container prerequisites (Podman required) and DevPod persistent workspace prerequisites (`uf setup` configures the docker-type provider aliased as `podman`). Add note about the `devpod up` failure diagnostic hint (`uf doctor` / `uf setup`). File: `content/docs/reference/sandbox.md`

## 2. Update blog post limitations

- [x] 2.1 [P] Update the "Current Limitations" section in `content/blog/sandbox-isolation.md` (line 147): refine the "Podman required" limitation to clarify it applies to ephemeral containers only, and that DevPod persistent workspaces use a configured provider that handles runtime resolution automatically. File: `content/blog/sandbox-isolation.md`

## 3. Add changelog entry

- [x] 3.1 [P] Add a changelog entry to `content/docs/changelog/_index.md` documenting: (a) removed standalone `podman` binary pre-flight check for DevPod workspaces, (b) added diagnostic hint on `devpod up` failure directing users to `uf doctor` and `uf setup`. File: `content/docs/changelog/_index.md`

## 4. Verification

- [x] 4.1 Run `npm run build` to verify the site builds without errors
- [x] 4.2 Verify constitution alignment: all four principles assessed as N/A (documentation-only change — no hero functionality, artifact interfaces, machine-parseable output, or testability implications)
