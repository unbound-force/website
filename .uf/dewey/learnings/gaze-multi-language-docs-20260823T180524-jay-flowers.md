---
tag: gaze-multi-language-docs
author: jay-flowers
category: pattern
created_at: 2026-08-23T18:05:24Z
identity: gaze-multi-language-docs-20260823T180524-jay-flowers
tier: draft
---

During the spec review phase of the gaze-multi-language-docs OpenSpec change, the review council consistently identified cross-page consistency gaps that the original spec missed. The Guard agent found that content/docs/team/gaze-tester.md and content/docs/projects/_index.md still said "for Go" when the project page was being updated to "Go-native with multi-language support." The Tester agent identified that the tester guide's side effect count "30+ types" would become inconsistent with the project page's expanded "48+ types" taxonomy. These cross-page consistency findings were auto-fixed by adding additional tasks and spec requirements. The pattern: when updating a primary documentation page, always grep for the old framing across all related pages (team pages, index pages, guide pages) to ensure the narrative is consistent. The grep command `grep -r "for Go" content/` catches most of these.
