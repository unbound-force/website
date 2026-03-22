---
title: "Getting Started: Product Manager"
description: "Use Mx F for sprint management, metrics dashboards, impediment tracking, coaching, and retrospectives in the Unbound Force workflow."
lead: "Monitor team health, facilitate retrospectives, and drive continuous improvement with Mx F."
date: 2026-03-22T00:00:00+00:00
draft: false
weight: 60
toc: true
---

## Using Mx F

[Mx F](/docs/team/mx-f/) is the Manager hero -- the Flow Facilitator and Continuous Improvement Coach. Mx F helps teams understand their development process, identify bottlenecks, and improve through data-grounded coaching and structured retrospectives.

Mx F has one important rule: **it never prescribes solutions**. It guides the team to discover their own path forward through reflective questioning and data-driven observations.

### The `mxf` CLI

The `mxf` command-line tool provides 7 subcommands for sprint management:

| Command          | Purpose                              |
| ---------------- | ------------------------------------ |
| `mxf collect`    | Gather metrics data from the project |
| `mxf metrics`    | Display current metrics              |
| `mxf impediment` | Track and manage blockers            |
| `mxf dashboard`  | Show the interactive dashboard       |
| `mxf sprint`     | Manage sprint lifecycle              |
| `mxf standup`    | Facilitate daily standups            |
| `mxf retro`      | Facilitate retrospectives            |

Install via Homebrew (included with the `unbound` package):

```bash
brew install unbound-force/tap/unbound
```

## Reading the Dashboard

The Mx F dashboard (`mxf dashboard`) provides a real-time view of team and project health across several key metrics.

### Velocity

Sprint throughput measured in completed tasks or story points per sprint. Velocity trends help with sprint planning -- if velocity has been stable at 20 points for 3 sprints, committing to 30 points in the next sprint is a warning sign.

### Quality Trends

Quality metrics from Gaze over time:

- **Contract coverage trajectory**: Is the team writing better tests? A rising trend means tests are increasingly verifying the right things.
- **CRAP score trend**: Are new functions being written with lower complexity? A declining CRAPload count means the codebase is getting safer.
- **Over-specification count**: Are tests becoming less brittle? A declining count means tests depend less on implementation details.

### Hero Utilization

Which heroes are active and how workload distributes across the team:

- Which heroes are being invoked most frequently
- How many review iterations are needed before approval (review efficiency)
- How many Gaze quality reports are generated per sprint

### CI Health

Build reliability over time:

- CI pass rate (percentage of builds that succeed)
- Average build duration trends
- Flaky test identification (tests that fail intermittently)

## Coaching and Retrospectives

Mx F facilitates team improvement through three coaching techniques and a structured retrospective protocol.

### Coaching Techniques

**5 Whys**: When a problem surfaces, Mx F asks "Why?" iteratively to reach root cause. Example:

> _PRs are getting REQUEST CHANGES frequently._
> Why? Code doesn't follow conventions.
> Why? Developers don't check before submitting.
> Why? Convention pack rules aren't clear.
> Why? Rules lack concrete examples.
> **Action**: Add examples to the convention pack, assign owner and deadline.

**Mirroring**: Mx F reflects the team's words back for clarity. "You mentioned CI is 'flaky.' What does 'flaky' mean specifically in your context?"

**Probing**: Open-ended questions that explore problem edges. "What changed since the last sprint?" "If you could fix one thing, what would it be?" "How would you know if this improvement worked?"

### Retrospective Protocol

Retrospectives follow 5 phases:

1. **Data Presentation**: Mx F reads metrics and presents key trends -- velocity, quality, review efficiency, CI health. Highlights changes from the previous sprint and reviews the status of prior action items.

2. **Pattern Identification**: The team identifies recurring themes. Mx F asks: "What patterns do you notice?" and "What surprised you?"

3. **Root Cause Analysis**: For each identified pattern, Mx F applies the 5 Whys technique to distinguish symptoms from root causes.

4. **Improvement Proposals**: The team proposes improvements. Each proposal must include a measurable success criterion -- "How will we know this worked?"

5. **Action Items**: Concrete action items with owner, deadline, and success criterion. Items are tracked with AI-NNN identifiers and followed up in the next retrospective.

### What Mx F Does Not Do

Mx F stays in its lane:

- Does not write code (that's Cobalt-Crush)
- Does not run tests (that's Gaze)
- Does not make architectural decisions (that's The Divisor)
- Does not choose technologies
- Does not assign blame

When a technical question arises, Mx F redirects: "That's a question for Cobalt-Crush. I can help analyze the patterns in our metrics."

## Integration with Other Heroes

Mx F is stage 6 (measure) in the [hero lifecycle](/docs/getting-started/common-workflows/#new-feature-end-to-end) -- the final stage that captures a metrics snapshot after each completed workflow.

### Mx F and Muti-Mind

Backlog velocity data informs priority decisions. When Mx F observes that velocity is declining, Muti-Mind can factor this into priority scoring -- perhaps reducing the number of high-effort items in the next sprint.

### Mx F and Gaze

Quality metrics from Gaze feed directly into Mx F's dashboards and coaching observations. Rising CRAP scores or declining contract coverage become coaching discussion topics in retrospectives.

### The Measure Stage

After a feature is accepted (stage 5), Mx F:

1. Collects a metrics snapshot capturing velocity, quality, review efficiency, and CI health
2. Updates the dashboard with the latest data
3. Identifies improvement opportunities for the next retrospective

## Next Steps

- Read [Common Workflows](/docs/getting-started/common-workflows/) to see how measurement fits the full lifecycle
- Explore the [Mx F](/docs/team/mx-f/) team page for the complete persona details
- Try `mxf dashboard` to see the current state of your project metrics
