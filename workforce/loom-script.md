# Loom demo script — Hormozi GTM Strategist workforce

**Target length:** 5 minutes (hard cap)
**Audience:** AI Ops Bootcamp judges. They've seen 50-100 submissions. They remember at most one thing per submission.

**The one thing you want them to remember:** *"The GTM agent that refuses the obvious answer and routes to the real constraint."*

---

## Why this script structure

Codex's narrative review flagged three traps that 99% of "I built an AI agent" demos fall into:
1. **Open with the architecture, not the output.** Viewers lose attention before they see the magic moment.
2. **Make the demo case feel engineered ("you built the maze").** Reviewer thinks: "of course it works on the case you designed it for."
3. **Frame the v1→v2→v3 iteration as failure, not hardening.** Reviewer thinks: "this person broke their own thing twice."

This script counter-engineers all three.

---

## Pre-recording checklist

- [ ] Workforce v5 is live. URL: https://app.relevanceai.com/workforce/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/0bae6bf7-18ca-48ce-a69b-548720551fa0/build
- [ ] Eval v5 results page is open in a second tab (for the eval table moment)
- [ ] One clean browser window, no Slack/email notifications
- [ ] Loom set to record screen + camera bubble (small, bottom-right)
- [ ] Demo input copied to clipboard for fast paste

---

## The script (5 min total)

### 0:00–0:20 — Cold open (no preamble, no name introduction)

> "Most AI agents just answer the question they're asked. This one refuses to. Watch what happens when I paste a real B2B SaaS problem."

**[On screen:** workforce trigger page, blank input.**]**

**Why this works (Codex feedback):** Skips the "Hi, I'm Aden" intro — judges have seen 50 of those. Starts with what the agent *does differently*. Earns the next 30 seconds.

---

### 0:20–1:30 — The diagnosis moment

**[Paste the demo input from `workforce/demo/input.md`.]**

> "$2K/month new tier, 40 demos, 7.5% conversion, prospects love it then go quiet, marketing wants more case studies, sales wants to drop the price. That's the surface. Now watch."

**[Click trigger. Wait. Workforce starts running.]**

**[As Triage's diagnosis appears on screen, narrate over it:]**

> "Triage refuses to recommend lower prices or more case studies. Instead it diagnoses an upstream Offer constraint — specifically a Perceived Likelihood gap. The buyer doesn't believe the outcome enough yet. Dropping price won't fix that. Adding generic case studies won't either."

**[Pause on the Diagnosis section. Highlight (if possible) the key line. The screenshot moment.]**

**Why this works:** This is the sit-up moment. Codex's exact recommendation: *"Dropping price will probably make this worse because the buyer doesn't believe the outcome enough yet; reduce perceived risk before reducing price."* That's the punchline. Make it the moment.

---

### 1:30–2:30 — The rejected paths

> "What makes this worth building is not that it picked Offer. It's that it explicitly rejected the others. Sales? Offer is doing its job — the demo lands. Pricing? Capacity isn't the constraint. Leads? Demo volume is fine. It considered each, named the override signals, and committed."

**[Scroll to the Pre-routing scan section. Show the signals listed.]**

> "And it surfaced an ICP risk on the side — flagging that if the prospects are concentrated in one specific segment, the Offer fix might miss the real issue. That's a second-order check Hormozi doesn't have a named framework for. The workforce has it as a sixth specialist."

**Why this works (Codex feedback):** Defuses "you built the maze." The agent explicitly shows its work — names what it considered, names what it ruled out, names what it flagged for follow-up. Reviewers stop suspecting cherry-picking.

---

### 2:30–3:30 — The specialist's substance

**[Switch to the Offer Specialist's output section. Scroll to the Value Equation scoring table.]**

> "The Offer Specialist scores the four Value Equation variables 1-10. Perceived Likelihood at 3. That's not opinion — that's a diagnostic. Then it recommends risk reversal: a paid POV, a champion enablement kit, a 30-day quickstart guarantee. Specific moves, not platitudes."

**[Highlight the 'Don't do this' section.]**

> "And it explicitly tells the user: don't drop the base fee, don't just add more case studies. Those are the natural moves and they're both wrong."

**Why this works:** Shows the depth — the Value Equation isn't just name-dropped, it's applied with numbers. The "Don't do this" beat is unusual; most AI advice is additive ("here's what to do"). Subtractive advice ("here's what NOT to do") signals expertise.

---

### 3:30–4:00 — The Synthesizer wrap

**[Scroll to Synthesizer brief — Cross-Level Stress Tests + What to do this week.]**

> "Then the Synthesizer wraps with cross-level stress tests. What you can't skip. What would invalidate this diagnosis. What to do this week. The handoff isn't 'here's a strategy doc.' It's 'here are two things to start Monday.'"

**Why this works:** The Synthesizer's "What to do this week" is the screenshot-worthy artifact. It's what a real founder would actually use.

---

### 4:00–4:30 — The eval story

**[Switch tab to eval results.]**

> "I built this with eval-driven iteration. Eight test cases. Five rules each. The first prompt version missed a case — it routed a retention problem to leads. So I added override signals. Then it over-corrected and missed a sales case. So I added stay-put counterweights. The final version routes all eight correctly. This isn't 'I built it and tested it.' This is 'I built adversarial evals and let them harden the routing.'"

**[Show the eval results table — 8 cases, all routing correct.]**

**Why this works (Codex framing):** Reframes v1→v2→v3 as **failure-mode hardening**, not "I broke it twice then fixed it." Key phrase: *"adversarial evals around common GTM misdiagnoses."* That's the bootcamp-relevant insight.

---

### 4:30–4:55 — Quick architecture mention (DO NOT linger)

> "Eight agents on Relevance AI. Hub-and-spoke. Triage routes. Six specialists run in parallel threads. Synthesizer wraps. Each specialist has a knowledge base of source-text Hormozi passages they can search on demand. That's it. The story isn't the agent count. The story is that it refuses the obvious answer and routes to the real constraint."

**Why this works:** Architecture in 25 seconds, max. Codex was emphatic: don't dwell. The judge already saw the demo work.

---

### 4:55–5:00 — Close

> "Real GTM is a stack of competing diagnoses. Most AI tooling collapses that into 'here's a generic recommendation.' This one keeps the diagnosis discipline. Thanks for watching."

---

## What to cut if you have to cut

Codex's cut priority (most expendable first):
1. **The architecture tour (4:30–4:55)** — if you're over time, drop it entirely. The judge will infer architecture from what they saw.
2. **The Synthesizer wrap (3:30–4:00)** — keep "What to do this week" if you can; cut the cross-level stress tests if needed.
3. **The eval story (4:00–4:30)** — if cut, save 30 seconds. Risk: loses the bootcamp-relevant "I tested it adversarially" angle. Cut last.

What you absolutely cannot cut:
- The diagnosis moment (0:20–1:30)
- The rejected paths (1:30–2:30)

Those two sections ARE the demo.

---

## The sticky one-liner (for the submission email / cover note)

> *"The GTM agent that refuses the obvious answer and routes to the real constraint."*

Codex feedback: this is what judges remember in 6 months. The "8-agent Hormozi workforce" framing is forgettable. The "refuses the obvious answer" framing is sticky.

---

## What NOT to say in the Loom

- "I'm passionate about AI" or any variant
- "This was a fun project"
- Anything about agent count, source-text chunk count, knowledge-base size, or the 88% eval score *as a percentage*
- Hormozi quotes or framework recitations
- Any version of "this could be the next ChatGPT"
- "Multi-agent orchestration" without immediately tying it to the user outcome

---

## Recording notes

- Drink water before recording — your voice tightens at minute 4 otherwise
- One take. If you fluff the cold open, restart from 0:00 — the cold open IS the demo
- Watch the camera bubble — keep eye contact with the camera, not the screen
- Pace: 150-170 words per minute. Faster than you think
- End the recording with a 2-second pause after "thanks for watching" — gives the Loom a clean ending frame

---

## Pre-Loom sanity check

Before recording, run the workforce ONCE with the demo input to confirm:
- [ ] Triage correctly routes to Offer (not Sales / Pricing / ICP)
- [ ] Triage's diagnosis includes the line "Dropping price will probably make this worse" (or close variant)
- [ ] Offer Specialist Value Equation table has Perceived Likelihood as the lowest score
- [ ] Synthesizer's "What to do this week" has 1-2 concrete actions
- [ ] Total runtime under 90 seconds (otherwise the Loom drags during the live trigger)

If any of these fail on the pre-check, do NOT record. Tweak the prompts first.
