---
tag: command-docs
category: pattern
created_at: 2026-05-02T21:01:29Z
identity: command-docs-2
tier: draft
---

For website documentation changes that document upstream CLI commands, the upstream command files in the unbound-force/unbound-force repo's .opencode/command/ directory are available locally at ../unbound-force/.opencode/command/ and should be read directly before writing website documentation. Dewey does not always have these specific command files indexed. Reading the upstream source ensures content accuracy per Constitution Principle I and prevents fabricating features. The command-docs change successfully used direct file access to verify /review-pr (548 lines) and /agent-brief (406 lines) features before documenting them.
