# Tasks: .uf/ Directory Path Update

**Input**: Design documents from `/specs/018-uf-directory-paths/`
**Prerequisites**: plan.md (required), spec.md (required)

## Phase 1: Content Pages — Dewey Paths (US1)

**Goal**: Replace all `.dewey/` references with `.uf/dewey/` in content pages

- [x] T001 [P] [US1] Replace `.dewey/` → `.uf/dewey/` in `content/docs/getting-started/knowledge.md` (~12 occurrences)
- [x] T002 [P] [US1] Replace `.dewey/` → `.uf/dewey/` in `content/docs/projects/dewey.md` (~2 occurrences)
- [x] T003 [P] [US1] Replace `.dewey/` → `.uf/dewey/` in `content/docs/team/dewey.md` (~3 occurrences)
- [x] T004 [P] [US1] Replace `.dewey/` → `.uf/dewey/` in `content/blog/dewey-knowledge-retrieval.md` (~2 occurrences)
- [x] T005 [P] [US1] Replace `.dewey/` → `.uf/dewey/` in `content/docs/getting-started/developer.md` (~1 occurrence)

**Checkpoint**: `grep -rn "\.dewey/" content/ | grep -v specs/` returns zero results

---

## Phase 2: Content Pages — Other Tool Paths (US1)

**Goal**: Replace `.unbound-force/`, `.hive/`, `.muti-mind/` references in content pages

- [x] T006 [P] [US1] Replace `.unbound-force/` → `.uf/` in `content/docs/getting-started/developer.md` (~2 occurrences)
- [x] T007 [P] [US1] Replace `.unbound-force/` → `.uf/` in `content/docs/getting-started/common-workflows.md` (~2 occurrences)
- [x] T008 [P] [US1] Replace `.hive/` → `.uf/replicator/` in `content/docs/projects/replicator.md` (~1 occurrence)
- [x] T009 [P] [US1] Replace `.muti-mind/` → `.uf/muti-mind/` in `content/docs/getting-started/knowledge.md` (~2 occurrences)

**Checkpoint**: `grep -rn "\.unbound-force/\|\.hive/\|\.muti-mind/" content/ | grep -v specs/` returns zero results

---

## Phase 3: Internal Agent/Command Files (US2)

**Goal**: Replace all `.opencode/unbound/` references with `.opencode/uf/` in agent and command files

- [x] T010 [P] [US2] Replace `.opencode/unbound/` → `.opencode/uf/` in `.opencode/agents/cobalt-crush-dev.md`
- [x] T011 [P] [US2] Replace `.opencode/unbound/` → `.opencode/uf/` in `.opencode/agents/divisor-adversary.md`
- [x] T012 [P] [US2] Replace `.opencode/unbound/` → `.opencode/uf/` in `.opencode/agents/divisor-architect.md`
- [x] T013 [P] [US2] Replace `.opencode/unbound/` → `.opencode/uf/` in `.opencode/agents/divisor-guard.md`
- [x] T014 [P] [US2] Replace `.opencode/unbound/` → `.opencode/uf/` in `.opencode/agents/divisor-sre.md`
- [x] T015 [P] [US2] Replace `.opencode/unbound/` → `.opencode/uf/` in `.opencode/agents/divisor-testing.md`
- [x] T016 [P] [US2] Replace `.opencode/unbound/` → `.opencode/uf/` in `.opencode/agents/divisor-techwriter.md`
- [x] T017 [P] [US2] Replace `.opencode/unbound/` → `.opencode/uf/` in `.opencode/agents/divisor-pr.md`
- [x] T018 [P] [US2] Replace `.opencode/unbound/` → `.opencode/uf/` in `.opencode/command/review-council.md`
- [x] T019 [P] [US2] Replace `.opencode/unbound/` → `.opencode/uf/` in `.opencode/command/uf-init.md`
- [x] T020 [P] [US2] Replace `.opencode/unbound/` and `.unbound-force/` → `.opencode/uf/` and `.uf/` in `.opencode/command/cobalt-crush.md`
- [x] T021 [P] [US2] Replace `.unbound-force/` → `.uf/` in `.opencode/agents/cobalt-crush-dev.md`

**Checkpoint**: `grep -rn "\.opencode/unbound/\|\.unbound-force/" .opencode/ | grep -v specs/` returns zero results

---

## Phase 4: Infrastructure Files

**Goal**: Update `.gitignore` entry

- [x] T022 [US1] Replace `.dewey/` → `.uf/dewey/` in `.gitignore`

---

## Phase 5: Verification

**Goal**: Build succeeds and zero stale path references remain

- [x] T023 Run `npm run build` — zero errors
- [x] T024 Grep verification: zero results for all old patterns outside `specs/`

---

## Dependencies & Execution Order

- Phases 1–4 are independent and can run in parallel
- Phase 5 depends on all prior phases completing
- All [P] tasks within a phase can run in parallel (different files)
