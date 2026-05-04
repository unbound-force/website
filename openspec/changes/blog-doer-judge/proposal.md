## Why

No existing blog post covers the architectural rationale behind the Divisor Council's structural doer/judge separation — why review agents are physically prevented from modifying files rather than behaviorally instructed not to. This is a distinctive architectural decision that maps directly to Anthropic's finding about agents "confidently praising broken work" when doer and judge roles collapse.

GitHub Issue: #91

## What Changes

- Add a new blog post at `content/blog/why-your-ai-code-reviewer-cannot-have-write-access.md`
- Covers structural vs behavioral enforcement, layered feedback stack, tool access restrictions, the 3-iteration review cap
- Attributes Anthropic findings to their source (Liu article)

## Capabilities

### New Capabilities
- `blog-doer-judge`: Blog post on structural doer/judge separation in AI code review

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- New file: `content/blog/why-your-ai-code-reviewer-cannot-have-write-access.md`
- No changes to existing pages

## Constitution Alignment

Assessed against the **Unbound Force Website** constitution (.specify/memory/constitution.md).

### I. Content Accuracy

**Assessment**: PASS

The Guard agent's tool restrictions (write: false, edit: false, bash: false) are verifiable against the agent persona file. The 3-iteration review cap is verifiable against the review-council command. Anthropic findings attributed to Liu article.

### II. Minimal Footprint

**Assessment**: PASS

Pure Markdown blog post, no custom elements.

### III. Visitor Clarity

**Assessment**: PASS

The post serves visitors building AI review workflows by providing a clear architectural rationale with a memorable CTA.
