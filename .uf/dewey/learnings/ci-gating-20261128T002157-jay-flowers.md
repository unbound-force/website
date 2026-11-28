---
tag: ci-gating
author: jay-flowers
category: gotcha
created_at: 2026-11-28T00:21:57Z
identity: ci-gating-20261128T002157-jay-flowers
tier: draft
as_of: "website@1987bb7484c6fb63b570ce25c4584c95a9ca9dfa"
evidence:
  - "unbound-force/website#467"
  - "unbound-force#1194"
  - "unbound-force/.github#73"
---

GitHub Actions prepends `success() &&` to any job-level `if:` containing no status function, so `if: needs.scope.outputs.code == 'true'` means "run only when the detector job SUCCEEDED and said true" — a failed detector skips the gated job, and a job skipped by `if:` reports success for a required status check of the same name. When gating a required-check job behind a detector: (1) make the condition fail-closed by skipping only on positive chore classification with an explicit status function, e.g. `if: ${{ !cancelled() && !(needs.scope.result == 'success' && needs.scope.outputs.code == 'false') }}`; (2) land the required-check swap (aggregate ci-ok replacing the gated job as required check, ideally requiring both during transition) in a coordinated change; (3) demonstrate the skip row (throwaway chore PR: scope=success, code=false, Build skipped, CI OK green) and a forced detector failure before promoting the aggregate to required — the PR that introduces the pattern always takes the full-CI path itself, so the skip-green and detector-failure rows are otherwise never exercised. Without this, detector failure during the transition window opens the old gate instead of closing it: merges proceed with the gated check never having run.
