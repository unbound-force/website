---
tag: ci-gating
author: jay-flowers
category: pattern
created_at: 2026-11-28T01:46:46Z
identity: ci-gating-20261128T014646-jay-flowers
tier: draft
as_of: "website@d75b8adb45ab6752ea77a15ae57c01097df43707"
---

A CI aggregate check only enforces its gating contract once branch protection requires it — when a workflow change depends on a required-check switch owned by another repo (e.g. a safe-settings ruleset), land the tracking artifact for that switch in the owning repo at the same time as the workflow PR, or the aggregate ships dormant and its drift-catching value is unrealized. The commitment cannot live only in PR-body prose: the person executing the owning-repo change needs a durable, discoverable pointer (issue in the owning repo) that also carries the verification preconditions (e.g. website #469 gating the CI OK promotion on runtime truth-table evidence). Evidence: website PR #467's CI OK vs unbound-force/.github safe-settings website.yml; gaze#373 precedent.
