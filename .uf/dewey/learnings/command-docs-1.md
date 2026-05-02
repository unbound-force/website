---
tag: command-docs
category: gotcha
created_at: 2026-05-02T21:01:24Z
identity: command-docs-1
tier: draft
---

When OpenSpec proposals reference updating a "quick reference table" in common-workflows.md, the spec review should verify whether that table actually exists. The command-docs change referenced a non-existent quick reference table across three artifacts (proposal, spec, tasks), which was caught as a HIGH finding in spec review. The table needed to be created, not updated. Always verify the current state of the target file before writing spec requirements that assume existing content structures.
