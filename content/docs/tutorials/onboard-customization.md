---
title: "Getting Started with /onboard — Customizing Your Hero Experience"
description: "Step-by-step tutorial for using the /onboard command to capture your preferences and customize how Unbound Force heroes respond to your working style."
lead: "Tell the heroes what matters to you. /onboard captures your inspiration, interests, and objectives into a profile that every hero reads — so they work the way you work."
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 50
toc: true
---

## Prerequisites

Before running `/onboard`, confirm that your environment meets these requirements:

1. **Run `uf init` first.** The `/onboard` command writes your profile to `.uf/onboarding/profile.md`. The `.uf/` directory must exist, and `uf init` creates it along with the rest of your project scaffold.
2. **Have OpenCode running.** The `/onboard` command is an interactive interview conducted through your OpenCode session. Start OpenCode in your project directory before proceeding.

If you skip `uf init`, the onboarding interview will fail because the `.uf/onboarding/` directory does not exist. Run `uf init` first, then return here.

## Running /onboard

Type `/onboard` in your OpenCode session to start the interview. The command walks you through three phases — Inspiration, Interests, and Objectives — each designed to capture a different dimension of how you work.

### Phase 1: Inspiration

The first phase asks: **What inspires your approach to engineering? What projects, books, or ideas shape how you think about building software?**

This is not a quiz. There are no wrong answers. The heroes use your inspiration sources to calibrate their communication style and technical references. If you admire the Unix philosophy, heroes will favor composable, single-purpose designs. If you draw from domain-driven design, heroes will use bounded-context language in their proposals.

```text
> /onboard

Welcome! Let's build your hero profile.

Phase 1 of 3: Inspiration

What inspires your approach to engineering? Think about
projects you admire, books that shaped your thinking,
or ideas that guide how you build software.

> I admire the Go standard library for its simplicity.
> "A Philosophy of Software Design" by John Ousterhout
> changed how I think about complexity. I also draw from
> the Erlang/OTP model of fault tolerance — let it crash,
> supervise, restart.

Got it. Simplicity-first design, deep module philosophy,
and fault-tolerant supervision patterns. Moving on.
```

Answer in your own words. Write a sentence or a paragraph — the interview adapts to whatever level of detail you provide.

### Phase 2: Interests

The second phase asks: **What technical areas fascinate you? What do you want to learn more about?**

Heroes use your interests to prioritize explanations and suggest approaches that align with what you care about. If you express interest in observability, Gaze will surface metrics and tracing patterns more prominently. If you mention distributed systems, Muti-Mind will lean toward event-driven architectures when multiple approaches are viable.

```text
Phase 2 of 3: Interests

What technical areas fascinate you? What do you want
to learn more about?

> I'm deep into observability — structured logging,
> distributed tracing, SLO-based alerting. I also want
> to learn more about formal verification and property-
> based testing. WebAssembly interests me but I haven't
> built anything with it yet.

Noted. Strong observability background, curiosity about
formal methods and property-based testing, early interest
in WebAssembly. Next phase.
```

### Phase 3: Objectives

The third phase asks: **What are you trying to achieve with your current project? What outcomes matter most?**

This phase grounds the profile in your immediate goals. Heroes read your objectives before every session to understand what "done" looks like for you. Concrete objectives produce better hero behavior than abstract ones.

```text
Phase 3 of 3: Objectives

What are you trying to achieve with your current project?
What outcomes matter most?

> I'm building a CLI tool for compliance automation.
> The MVP needs to parse OSCAL catalogs, generate control
> assessments, and produce human-readable reports. Ship
> date is Q3. Quality matters more than speed — I'd
> rather slip a week than ship with flaky tests.

Clear. Compliance automation CLI, OSCAL-based, quality
over velocity, Q3 target. Profile complete.

Writing profile to .uf/onboarding/profile.md...
Done. Every hero will read this profile at session start.
```

## Understanding the Profile

After the interview completes, `/onboard` writes your profile to `.uf/onboarding/profile.md`. The file has two parts: YAML frontmatter for structured metadata, and a Markdown body for narrative content.

### YAML Frontmatter Fields

The frontmatter captures machine-readable metadata that heroes parse programmatically:

| Field | Purpose | Example |
|-------|---------|---------|
| `created` | Timestamp of initial onboarding | `2026-08-23T14:30:00Z` |
| `updated` | Timestamp of most recent update | `2026-08-23T14:30:00Z` |
| `version` | Profile schema version | `1` |
| `status` | Whether the profile is complete | `complete` or `draft` |
| `interview_phases_completed` | Which phases finished | `[inspiration, interests, objectives]` |

### Markdown Body Sections

The body contains four sections that heroes read as natural language:

- **Inspiration**: Your engineering influences, distilled into themes the heroes reference when proposing designs or explaining trade-offs.
- **Interests**: Your technical fascinations, used to weight explanations and surface relevant patterns.
- **Objectives**: Your project goals and success criteria, consulted before every planning and implementation session.
- **Hero Mapping**: Auto-generated section that maps your profile themes to specific hero behaviors. This section updates each time you re-run `/onboard`.

### Sample Profile

```markdown
---
created: 2026-08-23T14:30:00Z
updated: 2026-08-23T14:30:00Z
version: 1
status: complete
interview_phases_completed: [inspiration, interests, objectives]
---

## Inspiration

Simplicity-first design inspired by the Go standard library.
Deep module philosophy from "A Philosophy of Software Design"
by John Ousterhout. Fault-tolerant supervision patterns from
Erlang/OTP — let it crash, supervise, restart.

## Interests

Strong background in observability: structured logging,
distributed tracing, SLO-based alerting. Exploring formal
verification and property-based testing. Early interest in
WebAssembly.

## Objectives

Building a CLI tool for compliance automation. MVP scope:
parse OSCAL catalogs, generate control assessments, produce
human-readable reports. Target ship date: Q3 2026. Quality
over velocity — prefer slipping schedule to shipping flaky
tests.

## Hero Mapping

- **Cobalt-Crush**: Favor deep modules over shallow wrappers.
  Use Go standard library idioms. Inject dependencies for
  testability.
- **Muti-Mind**: Propose event-driven patterns where applicable.
  Prioritize observability hooks in architectural decisions.
- **The Divisor**: Weight code clarity and test coverage in
  reviews. Flag complexity that contradicts the simplicity-first
  principle.
- **Gaze**: Surface CRAP scores and coverage gaps. Align quality
  thresholds with the "quality over velocity" objective.
```

## How Heroes Use the Profile

Every hero reads your profile at the start of each session. The profile changes how heroes communicate, what patterns they propose, and which trade-offs they prioritize. Here are concrete examples.

### Cobalt-Crush: Implementation Style

**Without a profile**, Cobalt-Crush uses generic best practices:

```go
// ProcessCatalog handles catalog processing.
func ProcessCatalog(path string) (*Result, error) {
    data, err := os.ReadFile(path)
    if err != nil {
        return nil, err
    }
    // ... processing logic
}
```

**With your profile** (simplicity-first, deep modules, testability), Cobalt-Crush injects the file reader and builds a deeper API surface:

```go
// ProcessCatalog parses an OSCAL catalog from the given reader and returns
// a structured assessment result. The reader abstraction allows testing
// without filesystem access — aligned with the deep module philosophy
// where the interface is narrow but the implementation handles complexity.
func ProcessCatalog(r io.Reader, opts ...Option) (*Result, error) {
    cfg := defaultConfig()
    for _, opt := range opts {
        opt(&cfg)
    }
    // ... processing logic with cfg-driven behavior
}
```

### Muti-Mind: Architecture Proposals

**Without a profile**, Muti-Mind proposes standard layered architecture.

**With your profile** (observability interest, fault tolerance inspiration), Muti-Mind proposes architectures that include tracing hooks and supervision trees:

> "Given your interest in observability and Erlang-style supervision, I recommend structuring the pipeline as a supervisor that spawns worker goroutines per catalog. Each worker emits structured trace spans. If a worker panics, the supervisor logs the failure, restarts processing for that catalog, and continues. This aligns with your 'let it crash' principle while maintaining the observability you care about."

### The Divisor: Review Focus

**Without a profile**, The Divisor applies uniform review criteria.

**With your profile** (quality over velocity, simplicity-first), The Divisor adjusts its severity weighting:

> "This function has a CRAP score of 42. Normally I'd flag this as MEDIUM, but your profile states 'quality matters more than speed' and draws from simplicity-first design. Elevating to HIGH. Recommend splitting the validation logic into a pure function that returns a typed error, then testing that function independently."

### Gaze: Quality Thresholds

**Without a profile**, Gaze uses project-default coverage thresholds.

**With your profile** (quality over velocity), Gaze tightens its recommendations:

> "Your profile prioritizes quality over velocity. I recommend setting the coverage ratchet to prevent regression rather than targeting a fixed percentage. Current coverage is 78% — I'll flag any PR that drops below this baseline."

## Updating Your Profile

### Re-running /onboard in Update Mode

Run `/onboard` again at any time to update your profile. The command detects the existing profile and enters update mode instead of starting fresh.

1. Type `/onboard` in your OpenCode session.
2. The command displays your current profile and asks which phases you want to revisit.
3. Select one or more phases to update. Phases you skip retain their existing content.
4. Complete the selected phases with your updated responses.
5. The command writes the updated profile with a new `updated` timestamp.

```text
> /onboard

Existing profile found (created 2026-08-23).

Which phases do you want to update?
  [1] Inspiration
  [2] Interests
  [3] Objectives
  [a] All phases
  [q] Cancel

> 3

Phase 3 of 3: Objectives (updating)

Current objectives:
  Building a CLI tool for compliance automation...

What are your updated objectives?

> Same project, but scope expanded. We now need to
> support SSP generation in addition to assessments.
> Ship date moved to Q4. Quality bar unchanged.

Updated. Writing profile to .uf/onboarding/profile.md...
Done.
```

### Profile History Snapshots

Each time you update your profile, `/onboard` saves a timestamped snapshot in `.uf/onboarding/history/`. Snapshots let you track how your goals and interests evolve over time. Heroes read only the current profile — snapshots exist for your reference.

```text
.uf/onboarding/
├── profile.md                          # Current profile (heroes read this)
└── history/
    ├── profile-2026-08-23T143000Z.md   # Initial profile
    └── profile-2026-09-15T091500Z.md   # After objectives update
```

## Edge Cases

### Abandoned Interviews

If you stop the interview before completing all three phases, `/onboard` saves a partial profile with `status: draft` in the frontmatter. The `interview_phases_completed` field lists only the phases you finished.

Heroes still read draft profiles. They use whatever information is available and apply defaults for missing phases. You will see a reminder at the start of each session:

```text
Note: Your onboarding profile is incomplete (missing: objectives).
Run /onboard to finish the interview.
```

Resume the interview by running `/onboard` again. The command picks up where you left off — it does not repeat completed phases unless you explicitly choose to update them.

### Missing Heroes

Your profile works regardless of which heroes are installed. If you reference a hero that is not present in your project (for example, you mention Gaze but have not installed it), the Hero Mapping section still generates an entry for that hero. The entry is ignored at runtime and activates automatically if you install the hero later.

Remove a hero from your project and the profile continues to function. Add a hero and it reads your existing profile immediately — no re-onboarding required.

### Draft vs. Complete Profiles

A **draft** profile has `status: draft` and is missing one or more interview phases. Heroes read draft profiles but may ask clarifying questions during sessions to fill gaps. For example, if the Objectives phase is missing, Cobalt-Crush may ask "What are you trying to build?" before starting implementation.

A **complete** profile has `status: complete` and all three phases finished. Heroes read complete profiles without additional questions. Complete profiles produce the most consistent hero behavior because every hero has the full context from the start.

Upgrade a draft to complete by running `/onboard` and finishing the remaining phases. The status field updates automatically when all three phases are present.

## Further Reading

- [The Team](/docs/team/) — meet the five heroes and understand their roles
- [Getting Started](/docs/getting-started/) — installation, setup, and common workflows
