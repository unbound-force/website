---
tag: blog-tutorials-batch
author: jay-flowers
category: pattern
created_at: 2026-08-24T00:11:37Z
identity: blog-tutorials-batch-20260824T001137-jay-flowers
tier: draft
---

For the Unbound Force website, FT-001 (code block language identifiers) is a consistent review finding when generating multiple content files. Blog posts and tutorials that contain output examples, interactive terminal sessions, or MCP tool calls frequently use bare opening fences without a language ID. The fix is straightforward — use `text` for output blocks and terminal sessions, `bash` for shell commands, and explicitly label MCP tool calls as `text` with a contextual note like "Run this MCP tool call in your AI agent session" since readers cannot paste MCP function calls into a terminal. Additionally, GitHub Actions workflow examples in blog posts must use SHA-pinned action references (not mutable tags like @v4) — this was caught in the gaze-baseline-comparison blog post where the companion tutorial correctly used SHA pins but the blog used @v4/@v5 tags.
