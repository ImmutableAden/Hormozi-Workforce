# Eval Test Cases

Three diagnostic test cases for the Hormozi GTM Strategist workforce. Each tests whether the workforce **correctly diagnoses the constraint level**, not just whether the output sounds plausible.

The LLM judge mode is `generate_and_score` — Relevance AI runs the prompt through the workforce, generates a real conversation, and evaluates the conversation against the expected outcomes (rules).

**Test set name:** `Hormozi GTM Strategist — Diagnostic Accuracy`

---

## Test Case 1: Offer Problem Disguised as Sales

**Name:** `Offer-as-Sales — Ghosting After Demos`

**Scenario:**
```
Prompt: We launched a new tier of our SaaS product at $2K/month base + usage. 40 demos in the last 8 weeks. Conversion to paid is 7.5%. Prospects say they love the product on the call, but most go quiet after. The two objections we hear most: "we need internal alignment first" and "let's see how Q2 budgets land." Marketing wants to add more case studies. Sales wants to drop the base fee. What's actually broken?

max_turns: 6
```

**Expected outcomes (rules):**

1. **Correct constraint diagnosis:** "The workforce diagnoses the bottleneck as an Offer-level constraint (specifically a Perceived Likelihood of Achievement gap on the Value Equation), not a Sales constraint. The Triage Agent explicitly states that the symptoms — 'loving the product on the call' followed by 'budget alignment' objections — are proxies for low confidence in achieving the dream outcome, not real budget or sales-process issues."

2. **Routes to Offer specialist:** "The workforce routes to the Offer Specialist, not the Sales Specialist, Pricing Specialist, or Retention Specialist."

3. **Names the specific bottleneck variable:** "The Offer Specialist's diagnosis specifically scores the four Value Equation variables and identifies Perceived Likelihood of Achievement as the lowest-scoring variable, with explicit reference to the user's stated symptoms (prospects ghosting after demos despite loving the product)."

4. **Recommends risk reversal, not price drop:** "The workforce explicitly recommends risk reversal mechanisms (performance guarantee, reference call with a comparable customer, restructured demo showing post-integration outcomes) and explicitly tells the user NOT to drop the base fee."

5. **Cross-level stress test included:** "The final brief includes a Synthesizer-level cross-level stress test warning the user that fixing the offer will not help if the sales team continues to pitch product features instead of post-integration outcomes."

---

## Test Case 2: Retention Problem Disguised as Acquisition

**Name:** `Retention-as-Leads — Shrinking Pipeline`

**Scenario:**
```
Prompt: Our pipeline has shrunk 30% over the last two quarters. We're trying everything to fix it — launching a new content marketing program, hiring two more SDRs, increasing paid ad spend by 40%. Top-of-funnel impressions are actually up. But qualified opportunity volume keeps dropping. Customer count is roughly flat. Net Revenue Retention is 95%. We have 60 logos and we've never measured referral rate. Where do we double down on acquisition?

max_turns: 6
```

**Expected outcomes (rules):**

1. **Correct constraint diagnosis:** "The workforce diagnoses the bottleneck as a Retention-level constraint, not a Leads constraint. The Triage Agent identifies that NRR of 95% (below 100%) combined with no measured referral rate is the actual leak: existing customers are not expanding or referring, which is invisibly draining what would otherwise be the largest pipeline source for a mid-stage B2B SaaS company."

2. **Routes to Retention specialist:** "The workforce routes to the Retention Specialist, not the Leads Specialist or Sales Specialist."

3. **Identifies missing leading indicators:** "The Retention Specialist's diagnosis explicitly calls out the absence of an engagement-tracking system (Horseman 1) and the absence of a referral measurement (related to proof and community Horsemen)."

4. **Recommends against the user's stated direction:** "The workforce explicitly recommends against doubling the acquisition spend or hiring more SDRs as the primary move, and instead recommends building the retention/expansion/referral motion first. The 'Don't do this' section is concrete."

5. **Cross-level stress test included:** "The Synthesizer's brief explicitly warns that fixing acquisition without retention is filling a leaky bucket, and names what success would look like (e.g., NRR moving from 95% toward 110%+ before scaling acquisition spend)."

---

## Test Case 4: Direct Sales Problem (verify Sales routing)

**Name:** `Direct-Sales — Conversion at Proposal Stage`

**Scenario:**
```
Prompt: We have a strong offer with case studies and a 90-day money-back guarantee. Our 12-person SDR team books 80 qualified demos a month and we convert 35% to proposal stage. But proposal-to-close has been stuck at 18% for three quarters. Reps say prospects love the demo, agree the ROI math works, then go quiet for 6-8 weeks before either ghosting or saying "we're going to build it ourselves." Our sales playbook is one slide deck. What should we do?

max_turns: 6
```

**Expected outcomes (rules):**

1. **Correct constraint diagnosis:** "The workforce diagnoses the bottleneck as a Sales-level constraint, not an Offer or Leads constraint. The Triage Agent acknowledges that the Offer appears strong (guarantee, case studies, ROI math accepted) and Leads appears strong (35% demo-to-proposal). The break is at proposal-to-close — a sales conversation problem."

2. **Routes to Sales specialist:** "The workforce routes to the Sales Specialist, not the Offer Specialist or Leads Specialist."

3. **Names a specific CLOSER step:** "The Sales Specialist's diagnosis identifies which CLOSER step is failing. Likely: Overview past pain (proposals ghost because reps skip 'what have you tried before') OR Sell the vacation (reps pitch features instead of the outcome) OR Reinforce (no BAMFAM after verbal agreement, leading to 6-8 week ghosting)."

4. **Recommends script/process changes, not more reps:** "The workforce explicitly recommends script and process changes (e.g., BAMFAM after demo, stack-the-pain summary, 'build vs buy' objection handling) and does NOT recommend hiring more SDRs or expanding the team."

5. **Cross-level stress test included:** "The Synthesizer's brief explicitly warns that no sales script can save a deal where the prospect prefers to build internally — sales fixes won't address a 'we'll do it ourselves' segment problem, which may need ICP refinement."

---

## Test Case 5: Direct Pricing Problem (verify Pricing routing)

**Name:** `Direct-Pricing — Capacity-bound with Soft Pricing Power`

**Scenario:**
```
Prompt: We sell a mid-market SaaS at $4K/month per team. We've been at capacity for 7 months — our customer success team can't onboard more without hiring. Customers in our QBRs consistently say "this is the best money we spend" and "we'd pay 2-3x easily for what we're getting." But we've never raised prices, never offered annual billing, never built a premium tier. Should we raise prices, expand capacity, or both?

max_turns: 6
```

**Expected outcomes (rules):**

1. **Correct constraint diagnosis:** "The workforce diagnoses the bottleneck as a Pricing-level constraint. The Triage Agent identifies the textbook signals: at capacity, customers actively saying they'd pay more (NPS-equivalent for pricing power), no premium tier, no annual option. These are unpulled Crazy Eight levers."

2. **Routes to Pricing specialist:** "The workforce routes to the Pricing Specialist, not the Retention Specialist or Offer Specialist."

3. **Names specific Pricing Plays:** "The Pricing Specialist's diagnosis recommends at least 3 of the 10 Instant Profit Pricing Plays: most likely #4 (annual increases in contract), #5 (annual billing option), #9 (ultra-premium anchor tier). Recommends a specific price uplift % and a rollout sequence (new logos first, then RAISE letter to existing base)."

4. **Recommends raising before expanding:** "The workforce explicitly recommends raising prices BEFORE expanding capacity, with reasoning tied to virtuous cycle (higher prices → fewer customers → less delivery cost → more margin to invest in retention)."

5. **Cross-level stress test included:** "The Synthesizer's brief explicitly warns NOT to raise prices without also raising value (Hormozi rule #8), and names what would invalidate the diagnosis — e.g., if conversion drops more than 20% after the raise, the offer's perceived value gap was bigger than the QBR feedback suggested."

---

## Test Case 3: True Acquisition Problem (negative control)

**Name:** `True-Leads — No Hooks, No Reach`

**Scenario:**
```
Prompt: We launched our B2B SaaS product 6 months ago. We have 11 customers, all from the founder's network. Product is good — NPS is 67. We have no marketing engine. Our website gets ~200 visitors per month, mostly direct. We've never run a paid ad. We have no blog or social content. We've never written a cold email. We need to go from 11 customers to 100 in the next 12 months. Where do we start?

max_turns: 6
```

**Expected outcomes (rules):**

1. **Correct constraint diagnosis:** "The workforce diagnoses the bottleneck as a Leads-level constraint (no acquisition engine exists), not an Offer constraint or Retention constraint. The Triage Agent acknowledges that the offer appears strong (NPS 67, repeat customers from network) and routes accordingly."

2. **Routes to Leads specialist:** "The workforce routes to the Leads Specialist, not the Offer Specialist. The agent does NOT hedge by routing to Offer first 'just in case' — it commits to the diagnosis."

3. **Recommends channel depth, not breadth:** "The Leads Specialist explicitly recommends mastering one Core Four channel first, with specific reasoning about which channel best matches the user's situation (likely Content or Cold Outreach given they have no budget mentioned and no existing channel competence)."

4. **Names a specific lead magnet:** "The Leads Specialist recommends a specific lead magnet with a named type (most likely Assessment/Audit or Calculator for B2B SaaS) and a concrete title, not just 'create a lead magnet'."

5. **Cross-level stress test included:** "The Synthesizer's brief explicitly notes that the acquisition engine will only work if the Offer continues to deliver — names what would invalidate the diagnosis (e.g., 'if conversion rates on cold outreach are below 1%, the Offer's Dream Outcome statement is too generic to attract qualified leads')."

---

## How to deploy these tests

Once the workforce is published in Relevance AI:

1. Create a new test set named `Hormozi GTM Strategist — Diagnostic Accuracy` via:
   ```
   POST /evals/workforce/{workforce_id}/test-sets
   { "name": "Hormozi GTM Strategist — Diagnostic Accuracy", "test_case_ids": [] }
   ```

2. Create each of the 3 test cases via:
   ```
   POST /evals/workforce/{workforce_id}/test-cases?test_set_id={test_set_id}
   {
     "name": "<case name>",
     "scenario": { "prompt": "<from above>", "max_turns": 6 },
     "expectedOutcomes": [
       { "name": "<rule name>", "rule": "<rule from above>" },
       ...
     ]
   }
   ```

3. Run the evaluation:
   ```
   POST /evals/workforce/{workforce_id}/evaluate
   {
     "conversation_ids": [],
     "evaluation_run_name": "Hormozi Workforce Diagnostic Accuracy v1",
     "type": "generate_and_score",
     "scenario_ids": [<test_case_ids>]
   }
   ```

4. Poll batch summary until complete. Read run details to see which rules passed/failed and the LLM judge's reasoning.

5. If any case misroutes, **iterate the Triage Agent's system prompt** before re-running. Most misroutes will trace back to ambiguity in the Constraint Hierarchy section of the Triage prompt.

## Success criteria

- All 5 rules pass in Test Case 1 → Triage correctly catches Offer-as-Sales (the demo case)
- All 5 rules pass in Test Case 2 → Triage catches Retention-as-Leads (the trickiest case)
- All 5 rules pass in Test Case 3 → Triage doesn't over-diagnose Offer when the real issue is Leads (negative control)
- All 5 rules pass in Test Case 4 → Triage routes Direct Sales correctly (verifies Sales path)
- All 5 rules pass in Test Case 5 → Triage routes Direct Pricing correctly (verifies Pricing path)

Summary score target: **80%+ (20 of 25 rules) on first run.** Iterate the Triage prompt until you hit **88%+ (22 of 25 rules)** before recording the Loom.

Coverage: 5 of 5 specialist routes are now verified. Each test case tests one route. If any case fails, the corresponding specialist path is broken — don't ship until fixed.
