---
tag: sandbox-docs
category: pattern
created_at: 2026-05-02T22:47:33Z
identity: sandbox-docs-1
tier: draft
---

After completing all 6 OpenSpec changes for the Unbound Force website in a single session (cli-reference-and-positioning, command-docs, config-docs, critical-doc-fixes, gateway-docs, sandbox-docs), a clear pattern emerged for proactive spec review fixes. Every single change had the same CRITICAL finding: wrong constitution assessed (org-level vs website project). By the third change, I started fixing this proactively before running the review council, which eliminated the CRITICAL finding and saved a review iteration. Other recurring patterns that should be fixed proactively in future batches: missing content sources section in design.md, missing reference section scaffolding tasks, cross-references to pages that don't exist on the current branch, and unqualified upstream spec paths in content sources.
