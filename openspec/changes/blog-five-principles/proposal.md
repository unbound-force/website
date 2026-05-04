## Why

No existing blog post addresses the system-level architecture — how individual tools (Gaze, Dewey, /unleash, the Divisor) compose into a coherent system. Every existing post covers a single tool. This is the flagship "how the pieces fit together" post, using Yanli Liu's five convergence principles as an organizing framework. All five content review agents identified this as the highest-impact blog opportunity.

GitHub Issue: #89

## What Changes

- Add a new blog post at `content/blog/five-principles-every-ai-agent-harness-discovers.md`
- The post walks through Liu's five convergence principles (context beats instructions, plan/execute separation, feedback loops, forced incrementalism, codebase-as-documentation) with concrete evidence from the Unbound Force codebase
- Proper attribution to Liu article; honest about harness weight and "build to delete"

## Capabilities

### New Capabilities
- `blog-five-principles`: A blog post presenting Unbound Force's system architecture through the lens of five convergence principles discovered independently by OpenAI, Anthropic, and ThoughtWorks

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- New file: `content/blog/five-principles-every-ai-agent-harness-discovers.md`
- No changes to existing pages, navigation, or styles
- Cross-references to existing getting-started and team pages where appropriate

## Constitution Alignment

Assessed against the **Unbound Force Website** constitution (.specify/memory/constitution.md).

### I. Content Accuracy

**Assessment**: PASS

All five principles are sourced from the harness engineering analysis (temp/harness-engineering-analysis.md) and verified against the actual upstream repository. Each claim about Unbound Force's implementation is verifiable against AGENTS.md, convention packs, and agent persona files. Liu article data points are attributed to their source.

### II. Minimal Footprint

**Assessment**: PASS

Pure Markdown blog post with standard frontmatter. No custom HTML, CSS, JavaScript, or template overrides.

### III. Visitor Clarity

**Assessment**: PASS

The post serves visitors who are evaluating AI agent workflows by providing a structured framework for understanding the system architecture. The narrative arc (problem → principles → evidence → CTA) follows established blog post patterns.
