---
name: company-research-profile
description: Build a structured, skeptical company research profile (as a styled HTML artifact) ahead of a job application, interview, or meeting with someone at a target company. Use whenever the user names a company they're evaluating as a job target, asks to "research" or "look into" a company, wants prep before a call/interview with a named contact, or asks to compare their resume against a specific role. Also covers updating an existing profile with new findings (e.g. after a call happens) or extracting fit/questions from a profile already on file. Trigger this even if the user doesn't say "profile" or "skill" explicitly — e.g. "can you dig into Acme Corp before my call with their VP of Product" or "is this company remote-friendly" should trigger it.
---

# Company Research Profile

Builds a single self-contained HTML page combining public-source research with an honest, skeptical read on fit, sentiment, and open questions — meant to be read once, right before a call or before deciding whether to apply.

The output is a tool for *decision-making*, not a marketing brochure about the company. The tone throughout should be the same as a sharp, skeptical friend doing due diligence — never a recruiter's pitch.

## When to use this

- User names a company and wants it researched as a job target
- User has a call, interview, or intro scheduled with a specific person and wants prep
- User wants to check a company against a hard requirement (e.g. remote work) before spending more time on it
- User wants to compare their resume/background against a specific posted role
- User wants to update a profile after new information comes in (a call happened, a new posting appeared, an offer changed)

If the user has personal hard requirements or background on file (career level target, must-have constraints like remote-only, resume/CV), use them — don't ask the user to repeat context that's already known. If those constraints aren't known yet, ask before starting research; they change what counts as a blocker versus a nice-to-have.

## Before starting research: confirm scope

Ask only what's actually missing (don't ask about things already known):
1. **Company name** and, if known, the **specific role/req** being evaluated.
2. Is there a **specific person** the user is meeting? If so, get their name/title so a "who you're talking to" section can be built.
3. Should this include a **resume fit assessment**? Only do this if a resume is available (uploaded, on file, or in project files) — never fabricate qualifications or invent a fit score without one.
4. Any known **hard constraints** to check first (remote-only, minimum level, visa/location, comp floor). These become the first-filter criteria — if a hard constraint fails, say so plainly early rather than burying it under enthusiasm for other parts of the fit.

## Research checklist

Work through these in order. Use web_search / web_fetch for each — don't rely on training data for anything that could be stale (funding, headcount, product direction, open roles).

1. **Company snapshot**: founding year/place/founders, HQ and other offices, ownership structure (private/public, funding raised, investors, valuation if disclosed), CEO (name, tenure, background), headcount, revenue (disclosed or credible third-party estimate), one-line description of what it sells.
2. **The role**, if one is in view: exact title, exact location/remote language *as written in the posting* (don't paraphrase "hybrid" into "flexible" — quote it), reporting structure, core requirements, comp band if published.
3. **Product history**: the original founding bet, and — critically — the recent pivot or current strategic direction with dated launches/announcements. Companies rarely stand still; the last 12–18 months matter more than the founding story.
4. **Employee sentiment**: Glassdoor (or equivalent) overall score and sub-scores (comp, work/life balance, culture, career opportunity, management), % would recommend, and — always — the review count. A 4.8 on 16 reviews is a different fact than a 4.8 on 800; say so. Pull specific recurring pros and recurring complaints, not just the aggregate number. If a specific office/team has a notably different score than company-wide, note both.
5. **Traction**: named customers, recent partnership/deal announcements with dates, analyst recognition (e.g. Gartner), open-source reach if applicable. Distinguish "customer" from "pilot" from "case study" from "ecosystem partner" — don't let a press release blur these.
6. **The named contact**, if applicable: title, tenure and career trajectory at the company (a person who's grown internally over years is a different signal than a recent hire), any public statements relevant to the role/product.
7. **Resume fit**, if applicable and a resume is available: an honest verdict (Strong / Mixed / Weak — resist the pull toward "Strong" by default), concrete line-by-line mappings between resume experience and role requirements, and — equally important — the real gaps (domain mismatch, level/scope mismatch, comp uncertainty, company size mismatch). State gaps as things worth naming *in conversation*, not things to talk around.
8. **Interview process**, if data exists (Glassdoor interview reviews etc.): stages, difficulty rating, positive-experience rate, sample size.

## Cross-checking and skepticism (important)

- When sources disagree (headcount, funding, revenue), say so explicitly rather than picking the number that sounds best. Note it as a reason to treat the figure as directional.
- Distinguish company-wide marketing language ("we're remote-first," "no pivot risk") from what specific postings or reviews actually say. Marketing copy is not verification — flag the gap and recommend confirming directly.
- Recency matters more than volume: a 2021 review about a since-abandoned strategy is weaker evidence than a 2025-2026 review about the current one — note this explicitly rather than averaging them together as if equally relevant.
- Never let enthusiasm for domain overlap ("this looks just like my old job") overstate real fit — call out where the domain is genuinely adjacent-but-different (e.g. BI/analytics vs. API observability), even if it's a less exciting thing to write.
- If a hard constraint (usually remote-work status) is unclear or contradicted across sources, don't resolve the ambiguity in the company's favor. State it as unresolved and put it in the priority-questions list.

## Building the document

Use the HTML template at `assets/template.html` as the structural and style starting point — it has the exact CSS (IBM Plex Serif/Sans, paper/teal/gold palette, card and bar-chart components) and section placeholders used in prior profiles. Copy it, then:

- Fill every placeholder with real researched content; delete sections that don't apply (role section if no specific req is in view, person section if no named contact, fit section if no resume is available). Don't leave placeholder text in the output.
- Keep the **lede** short (3–5 sentences + 3–5 bullets) and lead with whatever fact is most decision-relevant — usually the hard-requirement finding (e.g. remote status), stated plainly, not softened.
- The **watchlist** section is for things worth quietly verifying, not accusations — frame everything there as an open question, never a conclusion about the company.
- The **priority questions** section should hold only 2–3 questions: the ones that must get asked if time runs short, usually the hard-constraint checks (remote confirmation, level/scope, comp).
- Write the **fit callout** (`.callout` div) as a specific, honest sentence about domain overlap — not generic enthusiasm.
- Sources section: link every non-obvious factual claim back to something fetchable; don't include a source you didn't actually check.

Deliver the result as a standalone `.html` file the user can open or share, named `<company-name>-company-profile.html` (lowercase, hyphenated — e.g. `acme-corp-company-profile.html`). Follow file-edit consent norms — if updating an existing profile already on file, confirm the specific changes before overwriting.

## Updating a profile after new information

When the user reports back after a call, or a posting changes, or new info surfaces:
- Read the existing profile first (don't rebuild from scratch).
- Update the specific facts that changed, and move resolved watchlist items either into confirmed facts (if resolved positively) or into a clearly-stated blocker (if resolved negatively) — don't leave stale open questions sitting next to answers that already exist elsewhere in the doc.
- If a hard constraint that was previously unresolved is now confirmed as a blocker, say so plainly at the top of the lede — don't bury a dealbreaker in a later section.
