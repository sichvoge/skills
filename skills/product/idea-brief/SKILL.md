---
name: idea-brief
description: >
  Use this skill when the user wants to write an idea brief, product idea brief,
  feature proposal, or idea document. Triggers include: "idea brief",
  "new idea", "feature proposal", "product idea", "idea doc", or any request to
  structure a product/feature idea for review or approval. Also trigger when the user
  says they need to pitch an idea, propose a feature, or write up a concept for
  stakeholder review. The output is always a Markdown (.md) file. Do NOT use for
  full PRDs, technical specs, or detailed design documents.
---

# Idea Brief Skill

Write a concise, compelling idea brief that frames a product or feature idea for
stakeholder review. The brief follows a structure inspired by the "Fix the Inputs"
framework: define the problem, argue the timing, describe what success looks like,
and optionally include references.

## Voice and style

The brief should read as clear, direct, confident prose. Short declarative sentences.
No hedging, no filler. Every sentence should carry weight. Prefer concrete examples
over abstract claims. Use the present tense to describe the current state and its
pain. Use future tense sparingly, only in the vision section.

Avoid: "we believe", "it would be nice if", "potentially", "this could help".
Prefer: "This creates X. The result is Y. There is no Z today."

## Interactive workflow

Walk the user through the brief section by section. For each section, ask targeted
questions, draft the section from their answers, and confirm before moving on. The
user may also paste bulk context at any point, in which case extract what you can
and confirm gaps.

### Step 1: Metadata

Collect the basics upfront:

- **Title**: What is the idea called? (short, descriptive)
- **BLUF** (Bottom Line Up Front): One sentence. What is this idea in plain language?
- **Status**: e.g. "Draft", "In review (approval required by [name])"
- **Author**: Who is writing this?
- **Version**: Start at 0.1 unless told otherwise
- **Aha! ticket or tracking link**: Optional. Ask if there is one.

Draft the metadata table and BLUF, then confirm.

### Step 2: Challenges today (= What problem are we solving?)

This is the core problem statement. Ask the user:

- Who is affected? Be specific about the personas (e.g. "platform operators" vs "executives").
- What is the current experience? Walk through the pain step by step.
- What workarounds exist today, and why are they insufficient?
- What is the cost of doing nothing? (lost time, lost deals, maintenance burden, etc.)

Draft this section as a tight narrative. Each paragraph should make one point.
The section should make the reader feel the problem is real, specific, and worth solving.

Push back if the problem statement is vague. Ask: "Can you give me a concrete
example of someone hitting this problem? What did they do, and what went wrong?"

### Step 3: Why now

This section argues timing. The problem may have existed for a while, so why solve
it now? Ask the user:

- What has changed recently? (new strategy, new product launch, market shift, customer pressure)
- What upcoming event or deadline creates urgency? (summit, release, competitive move)
- What gets worse if we wait?

Draft this as 2-3 paragraphs that build the case for urgency. Connect the dots
between the problem, the strategic moment, and the cost of delay.

### Step 4: Vision and how success may look like (= What does success look like?)

Ask the user which structure fits their idea better:

**Option A: Vision-first (top-down).** Use when the user has a clear, ambitious
end-state in mind. Lead with the bold vision to get the reader excited, then
outline how to get there at a high level without prescribing detailed milestones.

Structure:
1. **The vision.** Paint the full future state. Be bold and concrete. Show what
   the world looks like when this is fully realized. Use specific scenarios to
   make it tangible (e.g. "A platform engineer registers a metric once and it
   appears in every export format automatically").
2. **Getting there.** Acknowledge the vision is ambitious. Describe a sensible
   starting point and general direction without going into milestone-level detail.
   Point to a PRD for the specifics.

Ask the user:
- What is the bold end-state? Describe it as if it already exists.
- What concrete scenarios show it working?
- Where would you start? What is the natural first step?

**Option B: Build-up (bottom-up).** Use when the user has a clear near-term fix
but the longer-term vision is still forming. Start with the smallest version that
removes the core pain, then expand outward.

Structure:
1. **Near-term**: What is the simplest version that removes the core pain?
2. **As it matures**: How does the capability expand? What new things become possible?
3. **The shift**: How does the workflow change for the people involved?

Ask the user:
- What is the minimum viable version of this? What does it do, concretely?
- If this works well, what does the next stage look like?
- How does this change the day-to-day for the people involved?

**Key principle: describe what the world looks like, not how to build it.** The
vision should make the reader want the future state without boxing in the team
that has to deliver it. Be concrete about outcomes and scenarios, not about
implementation. Leave the "how" for the PRD.

For both options, draft as a narrative. Use concrete scenarios to make the future
tangible, not abstract. Make the reader see it, not just understand it.

### Step 5: Example and visual references (optional)

Ask the user: "Do you have any examples from other products, competitors, or
internal tools that illustrate what you have in mind? Screenshots, links, or
even product names are all useful."

If they have references, list them with brief descriptions of what is relevant
about each one. If they do not, skip this section entirely.

### Step 6: Compile and deliver

Assemble the full brief as a Markdown file with this structure:

```
# [Title] | Idea

**BLUF:** [one-sentence summary]

| Field | Value |
|-------|-------|
| Status | [status] |
| Date | [month year] |
| Author | [name] |
| Version | [version] |
| Tracking | [link or "—"] |

## Challenges today

[problem narrative]

## Why now

[timing argument]

## Vision and how success may look like

[vision narrative — top-down (vision then path) or bottom-up (stages) per user choice]

## Example and visual references

[references, if any]
```

Save the brief as a file named `[slugified-title]-idea-brief.md`, using
whatever file-creation convention the current environment provides (e.g. the
outputs directory in Claude.ai, the working directory in Claude Code), and
make sure the user can open or download it.

## Constraints to surface (not a section, but a practice)

Throughout the conversation, listen for constraints the user mentions implicitly:
technical limitations, team capacity, dependencies on other teams, timeline
pressures, political considerations. Note these and reflect them back. They
should inform the problem statement and vision, not live in their own section.

If the user mentions a constraint that contradicts their vision, flag it:
"You mentioned [X]. Does that change what the near-term version looks like?"

## Quality checks before delivering

Before finalizing, verify:

- The BLUF is one sentence and a non-expert could understand it.
- Challenges today describes a specific, observable problem, not a feature gap.
- Why now makes a timing argument, not just a "this is important" argument.
- Vision is concrete and tangible, not abstract. If top-down, it leads with a bold end-state and outlines a path. If bottom-up, it shows a progression through stages.
- The brief is under 2 pages of prose (excluding references). If longer, tighten.
