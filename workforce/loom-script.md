# Loom demo script: Hormozi GTM Strategist workforce

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

### 0:00–0:20: Cold open (no preamble, no name introduction)

> "Most AI agents just answer the question they're asked. This one refuses to. Watch what happens when I paste a real B2B SaaS problem."

**[On screen:** workforce trigger page, blank input.**]**

**Why this works (Codex feedback):** Skips the "Hi, I'm Aden" intro. Judges have seen 50 of those. Starts with what the agent *does differently*. Earns the next 30 seconds.

---

### 0:20–1:30: The diagnosis moment

**[Paste the demo input from `workforce/demo/input.md`.]**

> "$2K/month new tier, 40 demos, 7.5% conversion, prospects love it then go quiet, marketing wants more case studies, sales wants to drop the price. That's the surface. Now watch."

**[Click trigger. Wait. Workforce starts running.]**

**[As Triage's diagnosis appears on screen, narrate over it:]**

> "Triage refuses to recommend lower prices or more case studies. Instead it diagnoses an upstream Offer constraint, specifically a Perceived Likelihood gap. The buyer doesn't believe the outcome enough yet. Dropping price won't fix that. Adding generic case studies won't either."

**[Pause on the Diagnosis section. Highlight (if possible) the key line. The screenshot moment.]**

**Why this works:** This is the sit-up moment. Codex's exact recommendation: *"Dropping price will probably make this worse because the buyer doesn't believe the outcome enough yet; reduce perceived risk before reducing price."* That's the punchline. Make it the moment.

---

### 1:30–2:30: The rejected paths

> "Picking Offer is the easy part. Watch what it ruled out. Sales? The demo lands. Offer is doing its job. Pricing? Capacity isn't the constraint. Leads? Demo volume is fine. It considered each, named the override signals, and committed."

**[Scroll to the Pre-routing scan section. Show the signals listed.]**

> "And it surfaced an ICP risk on the side, flagging that if the prospects are concentrated in one specific segment, the Offer fix might miss the real issue. That's a second-order check Hormozi doesn't have a named framework for. The workforce has it as a sixth specialist."

**Why this works (Codex feedback):** Defuses "you built the maze." The agent explicitly shows its work: names what it considered, names what it ruled out, names what it flagged for follow-up. Reviewers stop suspecting cherry-picking.

---

### 2:30–3:30: The specialist's substance

**[Switch to the Offer Specialist's output section. Scroll to the Value Equation scoring table.]**

> "The Offer Specialist scores the four Value Equation variables 1-10, with a reason for each. Perceived Likelihood at 3. Then it recommends risk reversal: a paid POV, a champion enablement kit, a 30-day quickstart guarantee. Concrete moves you could brief a sales team on tomorrow."

**[Highlight the 'Don't do this' section.]**

> "And it explicitly tells the user: don't drop the base fee, don't just add more case studies. Those are the natural moves and they're both wrong."

**Why this works:** Shows the depth. The Value Equation gets four numbers and a reason for each, treated as a real diagnostic instrument. The "Don't do this" beat is unusual; most AI advice is additive ("here's what to do"). Subtractive advice ("here's what NOT to do") signals expertise.

---

### 3:30–4:00: The Synthesizer wrap

**[Scroll to Synthesizer brief: Cross-Level Stress Tests + What to do this week.]**

> "Then the Synthesizer wraps with cross-level stress tests. What you can't skip. What would invalidate this diagnosis. What to do this week. The handoff lands as two things to start Monday. Concrete, dated, owned."

**Why this works:** The Synthesizer's "What to do this week" is the screenshot-worthy artifact. It's what a real founder would actually use.

---

### 4:00–4:55: How I built this

**[Switch tab to eval results.]**

> "I built this on Relevance AI in one day. Hub-and-spoke architecture: Triage diagnoses, six specialists with Hormozi source-text knowledge bases attached as searchable tools, Synthesizer wraps. The process was eval-driven from day one. Eight test cases, forty rules, five workforce iterations. Each one driven by a specific eval failure: v1 missed a retention case, v2 over-corrected to Offer, v3 added stay-put signals, v4 attached the knowledge bases, v5 took the Triage prompt to Codex CLI for cross-model adversarial review. Codex flagged the missing ICP layer above Hormozi's hierarchy in B2B SaaS. That became the 6th specialist. v5 currently grades 80% with one documented Direct-Sales regression that's the v6 work. Build stack: Relevance AI for the runtime, Claude Code for prompts and workforce config, Codex CLI for the second opinion. Two different model families disagreeing in useful ways."

**[Show the eval results table: 8 cases including Codex-A, B, C; highlight the Direct-Sales row honestly.]**

**Why this works (Codex framing):** Names the stack (Relevance AI + Claude Code + Codex), the process (eval-driven, cross-model adversarial review), the architecture (hub-and-spoke + knowledge tools), and the honest state (one known regression). Reframes v1→v5 as **failure-mode hardening**, never "I broke it twice then fixed it." Architecture is one breath, never a tour. Codex was emphatic on that. The bootcamp-relevant insight is the build discipline. The agent count is forgettable. Mentioning the Direct-Sales regression on camera is the move that separates this from a polished demo. Judges notice when a builder owns the gap instead of hiding it.

---

### 4:55–5:00: Close

> "Real GTM is a stack of competing diagnoses. Most AI tooling collapses that into 'here's a generic recommendation.' This one keeps the diagnosis discipline. Thanks for watching."

---

## What to cut if you have to cut

Cut priority (most expendable first):
1. **The Synthesizer wrap (3:30–4:00):** keep "What to do this week" if you can; cut the cross-level stress tests if needed.
2. **Trim "How I built this" (4:00–4:55):** if over time, drop the build-stack sentence (last line) first; drop the architecture sentence second. Keep the eval-driven process beat and the Codex cross-model review beat. Those are the bootcamp signal.
3. **The "How I built this" section as a whole:** only cut if you absolutely must. You lose the build-discipline angle, which is the thing that distinguishes this from a polished demo. Cut last.

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
- Stat-puffery: leading with agent count, source-text chunk count, knowledge-base row count, or "88%" as an opening hook (judges skim raw numbers; lead with what the thing *does*). It's fine to state a score when it's tied to a specific story. "v5 grades 80% with one documented regression" works because the regression *is* the story.
- Hormozi quotes or framework recitations
- Any version of "this could be the next ChatGPT"
- "Multi-agent orchestration" without immediately tying it to the user outcome

---

## Recording notes

- Drink water before recording. Your voice tightens at minute 4 otherwise.
- One take. If you fluff the cold open, restart from 0:00. The cold open IS the demo.
- Watch the camera bubble. Keep eye contact with the camera, not the screen.
- Pace: 150-170 words per minute. Faster than you think.
- End the recording with a 2-second pause after "thanks for watching". That gives the Loom a clean ending frame.

---

## Pre-Loom sanity check

Before recording, run the workforce ONCE with the demo input to confirm:
- [ ] Triage correctly routes to Offer (not Sales / Pricing / ICP)
- [ ] Triage's diagnosis includes the line "Dropping price will probably make this worse" (or close variant)
- [ ] Offer Specialist Value Equation table has Perceived Likelihood as the lowest score
- [ ] Synthesizer's "What to do this week" has 1-2 concrete actions
- [ ] Total runtime under 90 seconds (otherwise the Loom drags during the live trigger)

If any of these fail on the pre-check, do NOT record. Tweak the prompts first.
