# Hormozi GTM Strategist — a Relevance AI workforce

An 8-agent multi-agent system on [Relevance AI](https://relevanceai.com/) that diagnoses B2B SaaS go-to-market problems using Alex Hormozi's Constraint Hierarchy, plus a B2B-specific ICP/Positioning layer that sits above it.

Built as an AI Ops Bootcamp certification submission.

---

## The thing it does

You paste a real GTM problem. The workforce refuses to answer the surface question. It diagnoses the upstream constraint and routes to one of six specialists, who returns a scored diagnosis using the relevant Hormozi framework. A synthesizer wraps the output with cross-level stress tests, invalidation signals, and a "what to do this week" handoff.

The hook: most AI agents just answer the question. This one challenges the premise.

**Example.** Input: *"We have 40 demos, 7.5% conversion, prospects say they love the product but go quiet, marketing wants more case studies, sales wants to drop the fee. What's broken?"*

The workforce diagnoses an **Offer** constraint (Perceived Likelihood gap on the Value Equation), recommends risk reversal (performance guarantee, pilot structure) instead of price cuts, and explicitly warns against the obvious moves the user is considering.

---

## Architecture

```
                   ┌──────────────────────┐
                   │  Manual Trigger      │
                   │  (user chat input)   │
                   └──────────┬───────────┘
                              │ forced-handover (always-same threading)
                   ┌──────────▼───────────┐
                   │  TRIAGE AGENT (hub)  │ ← Constraint-Hierarchy diagnostician,
                   │                      │   refuses to answer surface questions
                   └──────────┬───────────┘
                              │
            tool-call edges (always-create-new threading)
            Triage picks exactly ONE specialist
            │     │     │     │     │     │
            ▼     ▼     ▼     ▼     ▼     ▼
         ICP   OFFER LEADS SALES RETN PRICE
        (meta) ▼
               │ (specialist returns response text)
               ▼
       Triage quotes specialist verbatim, then forced-handover →
                              │
                   ┌──────────▼───────────┐
                   │  SYNTHESIZER         │ ← cross-level stress tests,
                   │                      │   what would invalidate the diagnosis,
                   │                      │   what to do this week
                   └──────────────────────┘
```

**8 agents:**
- **Triage** (hub, Sonnet 4.6) — diagnoses which constraint level matters most
- **6 specialists** (each with a domain knowledge base attached as a `tool`):
  - ICP / Positioning (meta-level, added v4)
  - Offer (Value Equation, Grand Slam construction)
  - Leads (Core Four, lead magnets, hooks)
  - Sales (CLOSER framework, six-objection schema, scripts)
  - Retention (5 Horsemen, Crazy Eight LTV, Proof Checklist)
  - Pricing (10 Instant Profit Plays, RAISE framework)
- **Synthesizer** — wraps the specialist's brief with cross-level stress tests

**Knowledge bases:** Each specialist has a domain knowledge set of H2-chunked passages from Alex Hormozi's source material (`$100M Offers`, `$100M Leads`, the Lead Playbooks collection). Specialists `ALWAYS` run at least one knowledge search before producing a diagnosis. Total: ~88 source-text rows across 6 sets.

**Threading:** Specialists run in `always-create-new` threads. Triage extracts the specialist's response text and quotes it verbatim in its final message, which is what the Synthesizer reads.

---

## The eval-driven iteration story

The Triage prompt went through four iterations driven by structured eval failures.

| Version | Change | Eval result | What it broke / fixed |
|---|---|---|---|
| **v1** | Initial Constraint Hierarchy with soft "always check upstream first" guidance | 18/25 (72%) — 1 misroute, 2 infrastructure failures | Missed Retention-disguised-as-Leads (0/5 on that case) |
| **v2** | Added "anti-pattern checklist" with explicit override-up signals | 20/25 = 80% but Direct-Sales regressed to 1/5 | Over-overrode to Offer when stay-put-relevant evidence existed |
| **v3** | Added "stay-put signals" as counterweights to override-up triggers | 22/25 = 88% — all 5 routing decisions correct | Hit the 88% target; held under knowledge-base attachment |
| **v4** | Codex-driven fixes: tightened "no marketing engine" rule, added null-signal handling, soften proposal-to-close, ICP awareness + 6th specialist, model → Sonnet 4.6, autonomy → 5 | 16/20 = 80% on graded cases (4 of 8); 4 cases hit Relevance AI infrastructure errors | Adds 3 adversarial cases Codex constructed; one known regression on Direct-Sales (ICP override over-fired) |

This isn't "I broke it twice then fixed it." It's **failure-mode hardening** — adversarial evals around common GTM misdiagnoses, with each iteration adding a specific counter-pattern.

### v5 honest retrospective

The v5 build (the one this repo reflects) has a known regression:

- **Direct-Sales case dropped from 4/5 (v3) → 1/5 (v5).** The new ICP override in Triage v4 fires too aggressively on "we'll build it ourselves" without requiring the prospect to name an explicit segment. The workforce's *reasoning* is defensible (Codex's review specifically warned this signal is often ICP-related), but the eval rule expected Sales routing.
- **Documented fix path:** tighten the ICP override in Triage v5+ to require explicit segment naming. Codex-B (which has explicit "enterprise security teams" naming) would still route to ICP correctly; Direct-Sales (which doesn't name a segment) would stay with Sales.

This is the exact v1 → v2 regression pattern repeating: every new diagnostic rule has a price somewhere else. The honest engineering response is to keep tightening with adversarial evals, not pretend the rule is universally right.

### What graded cleanly in v5

- **Offer-as-Sales (DEMO case): 5/5 ✅** — the most important case held
- **Direct-Pricing: 5/5 ✅** — improved from 4/5 in v3 (Sonnet upgrade helped)
- **Codex-C (new adversarial — Delivery disguised as Pricing): 5/5 ✅** — proves the new Delivery-disguised-as-Pricing override works as designed

See [`docs/codex-review/synthesis.md`](docs/codex-review/synthesis.md) for the cross-model second-opinion review that drove v4.

---

## Repo structure

```
.
├── README.md                          # This file
├── .gitignore
├── workforce/                         # Source of truth for the build
│   ├── agents/                        # 8 agent system prompts
│   │   ├── 00-icp-positioning-specialist.md   (meta-level, added v4)
│   │   ├── 01-triage.md               (Sonnet 4.6, v4 prompt)
│   │   ├── 02-offer-specialist.md
│   │   ├── 03-leads-specialist.md
│   │   ├── 04-sales-specialist.md
│   │   ├── 05-retention-specialist.md
│   │   ├── 06-pricing-specialist.md
│   │   └── 07-synthesizer.md
│   ├── workforce/                     # Workforce graph config (hub-and-spoke topology)
│   │   └── config.md
│   ├── evals/                         # Eval test cases (8 scenarios × 5 LLM-judge rules)
│   │   └── test-cases.md
│   ├── demo/                          # Demo input for the Loom recording
│   │   └── input.md
│   ├── agent-ids.md                   # Live Relevance AI agent UUIDs
│   ├── eval-ids.md                    # Live eval test set IDs + version history
│   └── loom-script.md                 # 5-minute demo script with Codex-informed framing
└── docs/
    └── codex-review/                  # Cross-model second-opinion review (drove v4)
        ├── synthesis.md               # The cross-cutting writeup with action items
        ├── triage-challenge.txt       # Raw Codex Consult 1 output
        ├── architecture-challenge.txt # Raw Codex Consult 2 output
        └── narrative-review.txt       # Raw Codex Consult 3 output
```

---

## Live environment

- **Region:** `f1db6c`
- **Project:** Immutable (`f60582d1-7e8c-4875-9e3f-2082b1d51e74`)
- **Workforce ID:** `0bae6bf7-18ca-48ce-a69b-548720551fa0`
- **Workforce URL:** https://app.relevanceai.com/workforce/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/0bae6bf7-18ca-48ce-a69b-548720551fa0/build
- **Test set ID:** `acc895c4-29f2-4a86-8e66-85583dcd5a53`

Full agent and eval IDs in [`workforce/agent-ids.md`](workforce/agent-ids.md) and [`workforce/eval-ids.md`](workforce/eval-ids.md).

---

## What's deliberately NOT in here

Per Codex review, the workforce has three known architectural limitations that are NOT fixed in this version:

1. **"Pick exactly one specialist" is rigid.** Real GTM failures are often coupled (weak offer → poor sales → pricing resistance). The architecture forces a single-specialist route per query. Defensible design choice for clarity; would need restructure for production.
2. **Verbatim quote-back is brittle (LLM-as-transport-layer).** Triage quoting the specialist's brief into its final message depends on the LLM faithfully reproducing the text. Token pressure, paraphrase instinct, and formatting drift are all real risks. Relevance AI's threading architecture makes this hard to fix without restructuring.
3. **No retry / circuit-breaker logic.** Specialists returning malformed output triggers a "rerun the workforce" message. Production would want auto-retry and fallback specialist.

These are documented honestly because the bootcamp values eval-driven engineering thinking, not production-readiness claims.

---

## How to reproduce

Building locally requires:
- A Relevance AI account with API access
- The Hormozi skill source material (available at [github.com/garrytan/relevance-ai-hormozi-skill](https://github.com/garrytan/relevance-ai-hormozi-skill) — or wherever the user has it locally)

Reproduction is documented in `workforce/workforce/config.md`. The build sequence:
1. Create the 8 agents using the system prompts in `workforce/agents/`
2. Publish each agent
3. Create the 6 knowledge sets from the Hormozi source material (chunk by H2)
4. Attach each knowledge set to its specialist as `usage_type: tool`
5. Create the workforce with the topology described above
6. Run the eval test set in `workforce/evals/test-cases.md` to verify routing

---

## Credits

Source frameworks: Alex Hormozi (`$100M Offers`, `$100M Leads`, Lead Playbooks collection).

Build platform: [Relevance AI](https://relevanceai.com/).

Build assist: Claude Code (Anthropic), Codex CLI (OpenAI) for adversarial second-opinion review.

---

*Built for the Relevance AI Ops Bootcamp by [Aden Mann](https://github.com/ImmutableAden), Head of AI & Automation at Immutable.*
