---
tag: ci-gating
author: jay-flowers
category: gotcha
created_at: 2026-11-28T00:22:00Z
identity: ci-gating-20261128T002200-jay-flowers
tier: draft
as_of: "website@1987bb7484c6fb63b570ce25c4584c95a9ca9dfa"
evidence:
  - "unbound-force/website#467"
  - "unbound-force/gaze#373"
  - "unbound-force/replicator#185"
---

When a repository's required checks are managed centrally via org safe-settings (unbound-force/.github), a repo-local PR that makes a previously-required job skippable cannot bundle the branch-protection switch — the transition window (chore PRs waiting on a skipped required check; scope failures surfacing as hangs rather than red checks) must be closed by a follow-up org PR. Sequence that PR before or immediately after the repo PR (gaze precedent made it a before-merge prerequisite with a rollback runbook; replicator switched repo-level settings.yml in the same PR), and record the transition window in the workflow comment, not only the PR body, so the durable artifact matches observed behavior during the window.
