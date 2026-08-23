---
tag: gaze-multi-language-docs
author: jay-flowers
category: gotcha
created_at: 2026-08-23T18:05:09Z
identity: gaze-multi-language-docs-20260823T180509-jay-flowers
tier: draft
---

When running the /uf.unleash pipeline for documentation-only OpenSpec changes on the Unbound Force website, uncommitted work is lost across session breaks. The pipeline creates openspec artifacts and modifies content files but does not commit them until the final /uf.finale step. If a session is interrupted after implementation and code review pass but before committing, all work must be recreated from scratch. A mitigation pattern is to commit spec artifacts immediately after creation (with a "wip: openspec artifacts" commit) and commit implementation changes after each phase checkpoint. This prevents the full-recreation scenario that occurred during the gaze-multi-language-docs change, where the entire pipeline had to re-run because the branch was clean despite completing through code review.
