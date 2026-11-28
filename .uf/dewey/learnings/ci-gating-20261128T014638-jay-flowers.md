---
tag: ci-gating
author: jay-flowers
category: pattern
created_at: 2026-11-28T01:46:38Z
identity: ci-gating-20261128T014638-jay-flowers
tier: draft
as_of: "website@d75b8adb45ab6752ea77a15ae57c01097df43707"
---

Fail-red skip-gate pattern: when a CI job that feeds a required check is gated on a classifier job's output, skip only on the classifier's success — `if: ${{ !cancelled() && !(needs.scope.result == 'success' && needs.scope.outputs.code == 'false') }}` — never a bare `outputs.code == 'true'` (GitHub prepends an implicit `success() &&`, so a failed classifier silently skips the build while the skipped required check reads satisfied). Pair it with an always-run aggregate check that re-derives the expected state via a `case` with an explicit `*` → fail arm; single-site drift in either half then fails red, never green. Evidence: website PR #467, gaze PR #373, unbound-force PR #1202. The org-canonical issues unbound-force#1194 / gaze#372 / dewey#251 still publish the vulnerable bare-output form and should be updated when touched next.
