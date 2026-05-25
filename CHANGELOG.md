# Changelog

All notable changes to the Hormozi GTM Strategist workforce.

## [v4 / v5] — 2026-05-25 — Codex-driven hardening

Cross-model adversarial review (OpenAI Codex CLI, three independent consults: Triage challenge, architecture challenge, bootcamp-narrative review) drove a substantial rewrite.

### Added

- **6th specialist: ICP / Positioning Specialist.** Sits at the meta-level above Hormozi's Constraint Hierarchy. Diagnoses segment-mismatched proof, wrong-buyer champion traps, Two-segment trap, Category-urgency mismatch, No-defined-ICP, and Geoffrey Moore Chasm patterns. Codex flagged ICP as the "biggest gap" in v3 — the missing root node above Offer in B2B SaaS.
- **`hormozi-icp-source` knowledge set.** 5 source-text chunks covering Starving Crowd, the 3 P's, four-part buyer qualification, five B2B ICP failure patterns, ICP diagnostic questions.
- **Pre-routing ICP override signals in Triage prompt.** When Triage detects segment-mismatched proof, "we'd build this ourselves" concentrated in one segment, buyer-economics mismatch, or wildly different sales-cycle metrics across segments, it routes to ICP or surfaces a cross-cutting ICP concern in the diagnosis.
- **Force-search instruction on all specialists.** "ALWAYS run at least one knowledge search before producing your diagnosis" — addresses Codex's finding that `usage_type: tool` knowledge can be silently skipped by agents that "think they already know."
- **Null-signal handling in Triage prompt.** Explicit "No explicit signal matched" pathway for vague inputs — prevents confabulation when format demands collide with sparse evidence.
- **Delivery-disguised-as-Pricing override.** Triage routes to Retention (with delivery-model focus) when capacity-bound symptoms combine with high per-account manual work, low gross margins, or low NRR despite happy customers.
- **3 adversarial eval cases** constructed by Codex: mature leaky bucket (would v3 misroute to Leads?), wrong-segment proof (would v3 misroute to Sales?), delivery model disguised as pricing (would v3 misroute to Pricing?). Total eval set now 8 cases × 5 rules.
- **`docs/codex-review/`** with full cross-model review writeup + raw Codex outputs.
- **`workforce/loom-script.md`** — 5-minute demo script with Codex-informed narrative framing (lead with output, defuse "you built the maze," reframe v1→v2→v3 as failure-mode hardening).

### Changed

- **Triage model: `relevance-cost-optimized` → `anthropic-claude-sonnet-4-6`.** Codex called the cost-optimised model on the load-bearing routing decision the "highest-risk cost saving" in the system.
- **All agents: `autonomy_limit: 3 → 5`.** Codex flagged 3 as too tight for Triage's route + execute + quote-back sequence; specialists now have headroom for knowledge search + diagnosis.
- **Tightened "no marketing engine" stay-put rule.** Now gated to early-stage companies (`<2 years old OR <50 customers`). Prevents the false-positive on mature companies with leaky retention masquerading as Leads gaps.
- **Softened the proposal-to-close → Sales stay-put rule.** v3 was too confident; v4 acknowledges enterprise proposal-to-close failures can have non-Sales root causes (security review, economic buyer access, business case construction).
- **Diagnostic rule less absolutist.** v3's "always check upstream first" reframed as a strong prior, not an iron law. v4 explicitly notes the hierarchy is not perfectly linear in enterprise B2B SaaS.

### Known limitations (NOT fixed in v4)

Documented in README and codex-review/synthesis.md for transparency:

- **"Pick exactly one specialist" rigidity.** Coupled GTM failures (offer-causes-sales-causes-pricing) get routed to a single specialist.
- **Verbatim quote-back brittleness.** Triage's "quote the specialist's brief verbatim" depends on the LLM faithfully reproducing text. Relevance AI threading architecture makes this hard to fix.
- **No retry / circuit-breaker logic.** Malformed specialist output triggers "rerun the workforce" — no auto-retry, no fallback specialist.

---

## [v3] — 2026-05-25 (earlier same day) — Stay-put signals fix

### Changed

- **Triage prompt: added stay-put signals.** v2 added override-up triggers that fixed Retention-disguised-as-Leads (0/5 → 5/5) but regressed Direct-Sales (5/5 → 1/5) by over-overriding to Offer when stay-put-relevant evidence was present.
- **v3 added counterweights:** stay-put signals for Sales ("we have case studies / guarantee / ROI accepted / playbook is one slide deck / build vs buy"), Pricing (capacity-bound + high willingness-to-pay), and others. Routing logic became discriminating rather than blindly overriding.

### Result

- 22/25 = 88% eval score
- All 5 routing decisions correct across the original test set
- Held the same score after knowledge base attachment

---

## [v2] — 2026-05-25 (earlier same day) — Override-up checklist

### Changed

- **Triage prompt: added "anti-pattern checklist".** Each routing decision (Leads, Sales, Pricing) gained explicit override-up signals (e.g. NRR < 100% overrides Leads → Retention).

### Result

- Fixed Retention-as-Leads case (0/5 → 5/5)
- Regressed Direct-Sales case (5/5 → 1/5) — Triage over-overrode to Offer when stay-put-relevant evidence existed
- Net: 21/25, didn't hit target

---

## [v1] — 2026-05-25 (earlier same day) — Initial build

### Added

- 7 agents (Triage + 5 specialists + Synthesizer) on Relevance AI
- Hub-and-spoke workforce topology with tool-call edges + forced-handover
- 5 eval test cases (Offer-as-Sales, Retention-as-Leads, True-Leads, Direct-Sales, Direct-Pricing)
- 5 knowledge sets (Offer, Leads, Sales, Retention, Pricing) attached as `tool`

### Result

- 18/25 = 72% on first eval run (2 infrastructure failures, 1 misroute on Retention-as-Leads)
- Surfaced the prompt engineering gap that v2/v3/v4 addressed
