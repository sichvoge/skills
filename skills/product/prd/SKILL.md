---
name: prd
description: >
  Use this skill when the user wants to write a PRD (Product Requirements Document),
  product spec, or feature spec. Triggers include: "PRD", "product requirements",
  "requirements doc", "feature spec", "write a spec", or any request to define
  detailed requirements for a feature or product milestone. Also trigger when the
  user says they need to spec out a feature, define requirements, or document what
  needs to be built. The output is always a Markdown (.md) file. Do NOT use for
  idea briefs, high-level proposals, or technical design documents.
---

# PRD skill

Write a structured PRD that defines what needs to be built for a specific milestone.
The PRD moves from the "what and why" (covered in the idea brief) to the "what
exactly and how we'll know it worked." It covers requirements, success metrics,
competitive context, and cross-functional implications.

## Voice and style

Same principles as the idea brief: clear, direct, confident prose. Short
declarative sentences. No hedging, no filler.

The PRD is more precise than the idea brief. Where the idea brief paints a
picture, the PRD draws the blueprint. Requirements should be specific enough
that engineering can estimate work and design can start wireframes. Avoid
vague requirements like "the system should be fast." Prefer "time granularity
options: day, week, month, quarter, year."

Use the present tense for current state, imperative for requirements ("the user
must be able to..."), and future tense sparingly.

## Interactive workflow

Walk the user through the PRD section by section. For each section, ask targeted
questions, draft the section from their answers, and confirm before moving on. The
user may also paste bulk context at any point, in which case extract what you can
and confirm gaps.

If an idea brief exists for this feature, ask the user to share it. The idea brief's
"Challenges today" and "Vision" sections provide the foundation, so the PRD does
not need to re-argue the problem. Instead, reference the idea brief and focus on
the specifics of this milestone.

### Step 1: Metadata

Collect the basics upfront:

- **Title**: Format as "[Feature name] [Milestone] | PRD" (e.g. "Platform Analytics M1 | PRD")
- **Status**: e.g. "Draft", "In review", "Awaiting approval"
- **Date**: Month and year
- **Author**: Who is writing this?
- **Aha! ticket or tracking link**: Optional. Ask if there is one.

Draft the metadata table, then confirm.

### Step 2: Milestones

This is a simple table that breaks the work into named milestones with target dates.
Ask the user:

- What milestones do you see for this feature? (e.g. "GA", "Beta", "M2")
- What are the target dates?
- Which milestone is this PRD focused on?

Draft as a table with two columns: Milestone and Targeted date. Make clear which
milestone the rest of the PRD covers.

### Step 3: What are the must-have requirements

This is the core of the PRD. It defines what needs to be built for the milestone.
Ask the user:

- What must a user be able to do? Frame as concrete questions the user should be
  able to answer (e.g. "How many Control Planes does my organization have?").
- What is the high-level scope? Which entities, domains, or surfaces are included?
- What is explicitly out of scope for this milestone?
- Are there future considerations worth noting, even if they are not committed?
- Are there data, consistency, or quality requirements?
- Are there UX requirements or changes to existing workflows?
- Are there suggestions or open needs for design or documentation?

Draft requirements as a narrative with clear structure. Group related requirements
together. Use specific examples to make requirements unambiguous. Call out edge
cases and how they should be handled.

For out-of-scope items, be explicit about what is deferred and why. This prevents
scope creep and sets expectations.

Push back if requirements are vague. Ask: "Can you give me a specific example of
what the user would see or do? What data would they need?"

### Step 4: Success metrics

Define how the team will know the feature succeeded. Ask the user:

- What are the primary metrics? (adoption, usage, quality)
- What are the target numbers and timeframes?
- Are there secondary metrics worth tracking?
- How will these be measured? (instrumentation, surveys, manual tracking)

Draft as primary and secondary metrics with specific targets and measurement
windows (e.g. "60% of users interact with the feature within 30 days of GA").

Push back on metrics that are not measurable. Ask: "How would we actually track
this? Do we have instrumentation for it?"

### Step 5: Competitive analysis

Position the feature in the market. Ask the user:

- Are we leading, catching up, or lagging on this capability?
- Which competitors have similar features? What do they do well or poorly?
- Is this a differentiator or table stakes?

Draft as a brief assessment. This can be short, a paragraph or two. If there is
no meaningful competitive comparison, say so directly rather than padding.

### Step 6: Downstream / product-related implications

Identify cross-team dependencies and impacts. Ask the user:

- Which other teams or products are affected by this work?
- Are there API changes, data model changes, or infrastructure dependencies?
- Does any team need to be informed or coordinated with?

Draft as a list of impacted teams with what the implication is for each.

### Step 7: Naming implications

Ask the user:

- Are there any new names being introduced? (features, UI labels, data sources)
- Do any existing names need to change?
- Has PM and PMM aligned on naming?

If there are no naming changes, note that explicitly and move on.

### Step 8: Pricing & packaging

Ask the user:

- Does this feature affect pricing or packaging?
- Is it included in an existing SKU, or does it need a new one?
- Are there trial or gating implications?
- What is the recommendation for GA, and does it change post-GA?

Draft the rationale for the pricing approach, including what it avoids (e.g.
"avoids pricing model changes and sales enablement overhead") and any post-GA
considerations.

### Step 9: Open questions

Capture unresolved questions that need answers before or during implementation.
Ask the user:

- What questions are still open?
- Who owns each question?
- Are there any questions that are blocking vs. non-blocking?

Draft as a numbered list. Each question should include who raised it, the date,
and space for the answer. Questions that have been answered should show both the
question and the answer with the date resolved.

### Step 10: Compile and deliver

Assemble the full PRD as a Markdown file with this structure:

```
# [Title] [Milestone] | PRD

| Field | Value |
|-------|-------|
| Status | [status] |
| Date | [month year] |
| Author | [name] |
| Tracking | [link or "—"] |

## Milestones

| Milestone | Targeted date |
|-----------|---------------|
| [name] | [date] |

## What are the must-have requirements

[requirements narrative with scope, out of scope, UX details, edge cases]

## Success metrics

**Primary**
[metrics with targets and timeframes]

**Secondary**
[metrics with targets and timeframes]

## Competitive analysis

[market position assessment]

## Downstream / product-related implications

[cross-team impacts]

## Naming implications

[naming decisions or "No naming changes required."]

## Pricing & packaging

[pricing rationale and recommendations]

## Open questions

[numbered list of questions with owners and dates]
```

Save the PRD as a file named `[slugified-title]-prd.md`, using whatever
file-creation convention the current environment provides (e.g. the outputs
directory in Claude.ai, the working directory in Claude Code), and make sure
the user can open or download it.

## Constraints to surface (not a section, but a practice)

Same as the idea brief: listen for constraints mentioned implicitly throughout
the conversation. In a PRD, constraints often surface as infrastructure
limitations, team capacity, timeline pressure, or dependencies on other teams'
roadmaps. Reflect them back and make sure they are captured in the requirements
(as scope boundaries) or open questions (as unresolved blockers).

## Quality checks before delivering

Before finalizing, verify:

- Requirements are specific enough to estimate and design against.
- Out of scope is explicit and justified.
- Success metrics have concrete targets and measurement timeframes.
- Competitive analysis is honest, not aspirational.
- Downstream implications name specific teams and what they need to do.
- Pricing section addresses both GA and post-GA if relevant.
- Open questions have owners and dates.
- All section titles use sentence case.
