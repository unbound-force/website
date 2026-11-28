---
tag: ci-gating
author: jay-flowers
category: pattern
created_at: 2026-11-28T01:46:48Z
identity: ci-gating-20261128T014648-jay-flowers
tier: draft
as_of: "website@d75b8adb45ab6752ea77a15ae57c01097df43707"
---

An env-driven CI assertion run-block that takes all inputs via `env:` from `needs` context can have its entire input space verified locally with plain bash (a loop over the env combinations) before its first real run — do this whenever the block is a merge gate. actionlint cannot validate cross-repo output names or want-mappings, and throwaway-PR verification covers only one truth-table row at a time. Evidence: website PR #467's ci-ok job (SCOPE/CODE/BUILD case table, fully locally enumerable).
