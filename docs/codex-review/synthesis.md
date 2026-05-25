# Codex consult synthesis — 3-pass adversarial review

Three independent Codex (gpt-5.1, high reasoning) consults on Triage v3 prompt, workforce architecture, and bootcamp submission narrative. Below: the cross-cutting findings, ranked by impact and urgency.

---

## TIER 1 — Pre-Loom fixes (~15 min of work)

These are real bugs Codex caught. Fixing them before recording the Loom protects against the obvious "live demo blows up" risk.

### 1.1 The "no marketing engine" stay-put rule is too broad

**Codex finding (consult 1, Edge Case A):** A 6-year-old B2B SaaS with NRR 84%, 22% logo churn, 180 customers, no marketing engine, drying pipeline → the current v3 prompt would route to **LEADS** because "no marketing engine" triggers the stay-put rule. Correct answer is RETENTION.

**Why:** The stay-put rule was designed for the True-Leads case (early-stage, 11 customers, no acquisition yet). It doesn't distinguish "no marketing engine because we're 6 months old" from "no marketing engine because we've been coasting on referrals for 6 years and now they've dried up."

**Fix:** Tighten the rule to: *User describes the company as too new (e.g. "launched <2 years ago" OR "<50 customers") AND no functioning marketing channel*. The maturity gate prevents misrouting on the leaky-bucket-mature case.

### 1.2 No "no signals matched" null behavior

**Codex finding (consult 1, §4):** Vague inputs ("we're getting interest but not enough revenue, customers seem happy, sales feels slow") will fail to match any named signal. The strict "Pre-routing scan" format demand will cause the model to **confabulate signals** rather than admit ambiguity.

**Fix:** Add to the Triage prompt: *"If no named signal is explicitly present in the user's input, write 'No explicit signal matched' in the Pre-routing scan. Then route based on the strongest concrete evidence in the input only. Do not infer or manufacture signals to satisfy the format."*

### 1.3 ICP / positioning is the missing root node

**Codex finding (consults 1 & 3):** Hormozi's hierarchy (Offer → Leads → Sales → Retention → Pricing) omits ICP. In B2B SaaS, "right product, wrong segment" is one of the most common failure modes, and the workforce currently has nowhere to put it. Startup case studies + enterprise prospects + "build vs buy" objections is misdiagnosed as Sales when the answer is ICP/positioning mismatch.

**Fix for Loom (cheap):** Add a sentence to the Triage prompt: *"If the user's symptoms suggest ICP/positioning mismatch (e.g. proof exists but for the wrong segment, recurring 'we'd build this ourselves' from one segment, market timing/category urgency concerns), flag this explicitly in the diagnosis even if you route to Offer or Sales. ICP is not a specialist in this workforce; surface the gap so the user knows the routing is a best-fit, not a perfect match."*

**Real fix (post-Loom):** Add ICP/Positioning as a 6th specialist or a meta-check in Triage.

### 1.4 The Sales proposal-to-close stay-put rule is too confident

**Codex finding (consult 1, §2):** *"The break is specifically at proposal-to-close — that's a CLOSER conversation failure, not an Offer Perceived Likelihood failure"* is **false for enterprise SaaS**. Proposal-to-close can fail because of missing security proof, weak economic buyer value, bad implementation risk, no category urgency, procurement mismatch, or ROI credible only to the champion.

**Fix:** Soften the rule: *"The break at proposal-to-close MAY be a CLOSER failure; check first whether stronger Offer-is-OK signals are present (specifically: enterprise-relevant case studies, not just startup ones; named economic buyer engagement, not just champion enthusiasm)."*

---

## TIER 2 — Loom narrative rewrites (no code, just framing)

These changes only affect what you say and show in the 5-min recording. Codex consult 3 was specifically tasked here.

### 2.1 Open with the output, not the system

**Current plan:** Hook → paste input → show diagnosis → show specialist → show Synthesizer → show evals → show architecture.

**Codex's better arc:**
1. Paste the demo input
2. Show Triage **refusing** the surface answer (freeze on: "Do not cut price yet")
3. Show the **rejected paths** — why not Sales, why not Pricing, why not Leads
4. Show the specialist's one-week action plan
5. THEN evals
6. THEN architecture

Architecture comes last. Most viewers don't care about hub-and-spoke; they care that the agent did something a chatbot wouldn't.

### 2.2 The sit-up moment

**Codex's exact line:** *"Dropping price will probably make this worse because the buyer doesn't believe the outcome enough yet; reduce perceived risk before reducing price."*

This is sharper than "risk reversal recommended." If you can get the workforce to produce something close to this verbatim, that's the screenshot moment.

### 2.3 Reframe v1/v2/v3 as failure-mode hardening

**Codex's quote (worth using verbatim in the Loom):** *"I built adversarial evals around common GTM misdiagnoses. The first version obeyed surface framing. The second overcorrected. The final version added counter-signals so routing became discriminating instead of blindly overriding."*

Key phrase: **failure-mode hardening**. Don't say "v1 was bad, v2 was bad, v3 works."

Show one small table: Case | Expected route | v1 | v2 | v3 | What changed.

### 2.4 Defuse "you built a maze"

**Codex's blind-spot warning (consult 3, §8):** The demo case is too perfect — engineered to land on Offer. A tough reviewer thinks "you built the maze where you know the exit."

**Fix:** Add one sentence to the Loom: *"I also tested cases where the system should stay with Sales, Pricing, Leads, and Retention — because otherwise this is just an Offer hammer."*

That protects against the obvious criticism.

### 2.5 What to cut from the Loom

Cut (per Codex consult 3):
- Deep knowledge-base explanation
- Number of source rows (88 doesn't matter to anyone)
- Long explanation of all 7 agents
- Detailed Hormozi background / framework quotes
- H2 chunking architecture

Keep:
- Live diagnosis with the rejection moment
- Specialist's output (1-2 highlights, not the full diagnosis)
- One eval table (showing v1 → v3 progress on the misroute cases)
- One trace screenshot showing Triage → Specialist → Synthesizer

### 2.6 Hormozi as taxonomy, not authority

**Codex finding (consult 3, §5):** Leaning on Hormozi as branding makes the build feel like "guru wisdom bot." Better framing: *"I used Hormozi's hierarchy as the diagnostic taxonomy."* Don't quote him excessively. Don't make it sound cultish.

The load-bearing concept isn't Hormozi — it's **surface symptom vs root constraint diagnosis**. That's the durable insight.

### 2.7 The sticky line for memory

**Codex's exact phrasing (consult 3, §6):**

> *"The GTM agent that refuses the obvious answer and routes to the real constraint."*

Use this (or something close) as the one-sentence summary. Judges seeing 50-100 submissions will remember this; they won't remember "7 agents, 88 source chunks, 25 eval rules."

---

## TIER 3 — Production debt (NOT for Loom — for future)

These are real architecture issues but fixing them is outside Loom scope. Worth noting in the project memory for whoever picks this up next.

| # | Issue | Codex source | Why it matters | Effort |
|---|---|---|---|---|
| 3.1 | Cost-optimized model on Triage | Consult 2, §6 | Triage is load-bearing. Routing accuracy is the workforce's quality floor. Saving credits here is backwards. | 5 min — set model to Claude Sonnet or GPT-4o. Need to test eval doesn't regress. |
| 3.2 | Knowledge attached as `tool` not `instructions` for core doctrine | Consult 2, §4 | Specialists may not search when they think they already know. Core frameworks (Value Equation, Core Four, CLOSER) should be always-loaded. | 30 min — split knowledge sets into "doctrine" (instructions) and "examples" (tool). |
| 3.3 | "Pick exactly one specialist" is too rigid | Consults 1 & 2 | Real GTM failures are coupled — weak offer → poor sales → pricing resistance. The architecture forces one diagnosis. | 1-2 hr — allow conditional secondary routing or scoring across all five. |
| 3.4 | Verbatim quote-back is brittle (LLM-as-transport-layer) | Consult 2, §2 | Triage might paraphrase under token pressure, drop caveats, normalize quotes, etc. The Synthesizer's evidence-quality depends entirely on this surviving. | Hard — would require restructuring threading or adding structured-data passing. Relevance AI may not support this. |
| 3.5 | No retry/circuit-breaker logic | Consult 2, §7 | "Rerun the workforce" is demo-grade. Production needs auto-retry on transient failures + fallback specialist. | 1-2 hr — Relevance AI may not have native retry; would need to handle at Triage level. |
| 3.6 | No ICP/positioning specialist | Consults 1 & 3 | Missing root node above Offer. Many B2B SaaS failures are mis-categorized as Offer or Sales when they're ICP. | 2-3 hr — design + write + eval ICP specialist + update Triage prompt. |
| 3.7 | Demo input set is thin (5 cases) | Consult 3, §1 | 88% on 5 cases isn't strong proof. Add 10-15 more adversarial cases (including the Edge Cases A/B/C Codex constructed). | 1-2 hr per case to write + run eval. |
| 3.8 | `terminate-conversation` is hostile | Consult 2, §3 | Abrupt failure when autonomy_limit hits. No graceful degradation. | 5 min — change to `ask-for-approval` and add fallback handling. |

---

## What to act on RIGHT NOW vs queue for later

**Before recording the Loom (estimated 20 min):**
- Fix 1.1 (mature-leaky-bucket stay-put bug) — real misroute risk if anyone in the bootcamp throws an adversarial input at it
- Fix 1.2 (null-signal behavior) — vague input handling
- Add 1.3 (ICP-flag sentence) and 1.4 (soften proposal-to-close rule)
- Re-run the 5-case eval to verify these don't regress (~7 min)
- Optional: write the 3 Codex-constructed edge cases as new eval scenarios; running them would either confirm Codex's predictions (great Loom material: "I tried to break it after Codex review — here's what happened") or refute them (also fine, you've stress-tested)

**For the Loom narrative:**
- Restructure to lead with output, not architecture (§2.1)
- Add the "I also tested negative cases" defusing line (§2.4)
- Use the "failure-mode hardening" framing for v1→v2→v3 (§2.3)
- Use "the GTM agent that refuses the obvious answer" as the sticky line (§2.7)

**After the Loom (queue for the future):**
- Tier 3 items 3.1, 3.2, 3.6 are the highest-leverage (model, knowledge mode, ICP)
- 3.3, 3.4, 3.5 are deeper architecture work — only if this becomes a real product, not a bootcamp submission

---

## What Codex agreed with Claude on

For the record (since the skill flagged cross-model agreement is informative):
- The architecture is **defensible**, not optimal. Codex didn't say "tear it down."
- The eval-driven iteration story is **good if framed well**.
- The 88% number is **bootcamp-valid, not operator-valid** — fine for this purpose, weak for production claims.
- Hub-and-spoke + Triage refuses to answer is a real and memorable design choice — both Codex and Claude liked it.

## What Codex disagreed with Claude on

- **The Triage-doesn't-answer pattern.** Claude designed and praised this. Codex called it "ceremony" and proposes Triage as primary strategist with specialists as optional retrieval/critique tools. Worth a second think — Codex's argument is that diagnosis and prescription are tightly coupled in GTM, and forcing them apart introduces handoff failures without adding clarity.
- **Cost-optimized model on Triage.** Claude went along with the default. Codex says this is the highest-risk cost-saving in the system. Codex is probably right.
- **Knowledge as `tool` vs `instructions`.** Claude chose `tool` (search on demand). Codex says core doctrine should be `instructions`. Defensible disagreement; depends on whether you trust models to invoke search.
