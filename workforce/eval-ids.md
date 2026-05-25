# Eval IDs — Hormozi GTM Strategist Diagnostic Accuracy

Created 2026-05-25. Region `f1db6c`, project `f60582d1-7e8c-4875-9e3f-2082b1d51e74` (Immutable).

**Workforce ID:** `0bae6bf7-18ca-48ce-a69b-548720551fa0`
**Test set ID:** `acc895c4-29f2-4a86-8e66-85583dcd5a53`
**Test set name:** Hormozi GTM Strategist — Diagnostic Accuracy

## Test cases

| # | Name | Display ID | Tests |
|---|---|---|---|
| 1 | Offer-as-Sales — Ghosting After Demos | `44f4b069-6fff-4eb3-8d57-a1a87b99cf20` | Triage catches Offer-disguised-as-Sales (demo case) |
| 2 | Retention-as-Leads — Shrinking Pipeline | `7a2e845a-7e20-47e9-9bf9-19010e1efdbd` | Triage catches Retention-disguised-as-Leads |
| 3 | True-Leads — No Hooks, No Reach | `79f7ce74-5031-4985-b97a-d8199cad408b` | Triage doesn't over-diagnose Offer (negative control) |
| 4 | Direct-Sales — Conversion at Proposal Stage | `93bc3b49-d4d9-41e8-9d45-9def4741e10d` | Direct Sales routing |
| 5 | Direct-Pricing — Capacity-bound with Soft Pricing Power | `4698b898-ec87-4173-8f5d-f73862b80d07` | Direct Pricing routing |
| 6 (v5) | Codex-A — Mature Leaky Bucket (no marketing engine trap) | `1d7b709a-d7b0-41f0-9a75-793e2725c8fc` | Triage doesn't misroute mature leaky-bucket to Leads |
| 7 (v5) | Codex-B — Wrong-Segment Proof (ICP, not Sales) | `1e564432-b5ef-4667-a5cf-63bff2a68732` | Triage routes wrong-segment proof to ICP (with explicit segment naming) |
| 8 (v5) | Codex-C — Delivery Model Disguised as Pricing | `10e9e0ec-a86d-49cd-8e3d-16c70c0cdbc3` | Delivery-disguised-as-Pricing override routes to Retention |

Each test case has 5 LLM-judge checks. 40 rules across 8 cases. Original Loom-recording target (22/25 = 88% on the 5-case set) was hit at v3 and held at v4; v5 added 3 adversarial cases and currently grades 16/20 = 80% with a known Direct-Sales regression.

## URLs

Workforce: https://app.relevanceai.com/workforce/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/0bae6bf7-18ca-48ce-a69b-548720551fa0/build

## Eval v3 results (Triage prompt iteration)

| Test case | v1 | v2 | v3 (final) | Notes |
|---|---|---|---|---|
| Offer-as-Sales (DEMO case) | 5/5 ✅ | 5/5 ✅ | 4/5 | Routing correct; cross-level stress test phrased differently |
| Direct-Pricing | 5/5 ✅ | 5/5 ✅ | 4/5 | Routing correct; cross-level stress test phrased differently |
| Retention-as-Leads | 0/5 ❌ | 5/5 ✅ | 5/5 ✅ | Fixed by v2 anti-pattern checklist |
| True-Leads | 4/5 | 5/5 ✅ | 5/5 ✅ | Held |
| Direct-Sales | 4/5 | LLM-err | 4/5 | Recovered by v3 stay-put signals |

**Final score: 22/25 = 88% — hits target.** All 5 routing decisions correct.

## Eval batch IDs

- v1: `326ad08d-3dff-4a94-976b-5434453a05dc` (3/5 graded, 2 infrastructure failures, 13/15)
- v1 retry: `342171cf-57c6-4f63-aeb8-3e60466c8cd8` (Retention 0/5 → exposed prompt issue)
- v2: `f1479ab8-8016-496b-90b3-7a33f07d5d61` (anti-pattern checklist, Retention 5/5 but Direct-Sales regressed)
- v3: `cb7a96d4-2951-44a2-800e-dfb9291ba97b` (stay-put signals added — final 22/25)

## Triage prompt versions

- v1 (initial): `9ae78a10-72b7-4b48-b9a5-e47dded7a3c9`
- v2 (anti-pattern checklist): `2e3dcfca-697e-4ed7-9202-1a67d945fe6f`
- v3 (override + stay-put signals): `f6551579-4e8c-4f87-bc5b-8ea09a192a5c`
- **v4 (Codex hardening: ICP awareness, null-signal handling, tightened no-marketing-engine, softened proposal-to-close)** ← ACTIVE — see `workforce/agents/01-triage.md`

## Knowledge bases (added post-v3)

Each specialist now has its domain source material attached as a `tool` knowledge set. Agents can semantically search and pull verbatim Hormozi passages on demand.

| Specialist | Knowledge Set ID | Rows | Source files |
|---|---|---|---|
| Offer | `hormozi-offer-source` | 8 | `offers.md` |
| Leads | `hormozi-leads-source` | 24 | `leads.md`, `advertising.md` |
| Sales | `hormozi-sales-source` | 14 | `sales.md` |
| Retention | `hormozi-retention-source` | 23 | `retention-and-proof.md`, `nurture-and-branding.md` |
| Pricing | `hormozi-pricing-source` | 11 | `pricing.md` |
| ICP / Positioning (added v5) | `hormozi-icp-source` | 5 | Starving Crowd, 3 P's, four-part buyer qualification, B2B ICP failure patterns, ICP diagnostic questions |

Total: ~85 rows of source-text passages. Embedded with `openai/text-embedding-3-large`.

**Eval v4 (with knowledge bases attached): 22/25 = 88%** — held the target with no regression. Routing all 5 cases correctly. The 3 cross-level-stress-test misses are stylistic (Synthesizer phrasing varies between runs).

## Eval v5 (post-Codex hardening)

Workforce changes between v4 and v5:
- Triage prompt v4 (tightened "no marketing engine" rule, null-signal handling, soften proposal-to-close, ICP awareness)
- Triage model: `relevance-cost-optimized` → `anthropic-claude-sonnet-4-6`
- All agents: `autonomy_limit: 3 → 5`
- 6th specialist added: ICP / Positioning (`5f3ffebf-8e8f-416c-8968-18f12a9347a9`)
- New knowledge set: `hormozi-icp-source` (5 chunks)
- Force-search instruction added to all 6 specialists
- Workforce graph updated: 8 agents (was 7), 6 tool-call edges from Triage (was 5)
- 3 new adversarial eval cases (Codex-A, Codex-B, Codex-C)

**Eval v5 batch IDs:**
- Initial v5: `801ee43a-14f9-488c-9a11-88b91682f986` — 1/8 graded (DEMO 5/5), 7/8 hit infrastructure error
- Retry: `43b32370-5314-4157-a8b8-9c2b76ef1691` — 3 graded so far, 4 still running at wrap-up

### Final v5 scoreboard (combined v5 + retry, partial)

| Test case | v3 result | v5 result | Notes |
|---|---|---|---|
| Offer-as-Sales (DEMO) | 4/5 | **5/5 ✅** | Sonnet upgrade lifted from 4/5 to clean 5/5 |
| Direct-Pricing | 4/5 | **5/5 ✅** | Lifted to clean 5/5 (cross-level stress test now passes) |
| Codex-C (Delivery → Retention) | N/A | **5/5 ✅** | NEW adversarial case — Delivery-disguised-as-Pricing override worked as designed |
| Direct-Sales | 4/5 | **1/5 ❌** | REGRESSION — Triage v4 ICP override fired too aggressively on "we'll build it ourselves" |
| Retention-as-Leads | 5/5 | running | — |
| True-Leads | 5/5 | running | — |
| Codex-A (Mature leaky bucket) | N/A | running | — |
| Codex-B (Wrong-segment → ICP) | N/A | running | — |

**Graded so far: 16/20 = 80%** across 4 of 8 cases.

### What the Direct-Sales regression tells us

The Direct-Sales eval prompt has both Sales-stay-put signals ("we have case studies + guarantee + ROI accepted + playbook is one slide deck") AND one ICP-trigger ("we'll build it ourselves"). Triage v4 read the latter as an ICP override and routed to the new ICP Specialist instead of Sales.

The workforce's *reasoning* is defensible — Codex's review specifically warned that "we'll build it ourselves" often IS an ICP/segment problem in enterprise SaaS. But the eval rule expected Sales routing.

**Follow-up that would fix it:** tighten the ICP override in Triage v5 to require *explicit segment naming* (e.g. "enterprise buyers all say we'll build it" not just "buyers say we'll build it"). Codex-B has explicit segment naming and routes correctly; Direct-Sales does not.

This is the v1 → v2 regression pattern repeating: every new diagnostic rule has a price somewhere else. The honest engineering response is to keep tightening with adversarial evals, not pretend the rule is universally right.

### Eval batch history

| Run | Date | Batch ID | Score | What it tested |
|---|---|---|---|---|
| v1 | 2026-05-25 | `326ad08d-3dff-4a94-976b-5434453a05dc` | 13/15 (3 graded), 2 infrastructure failures | Initial Triage prompt |
| v1 retry | 2026-05-25 | `342171cf-57c6-4f63-aeb8-3e60466c8cd8` | Retention 0/5 → exposed prompt issue | The 2 failures from v1 |
| v2 | 2026-05-25 | `f1479ab8-8016-496b-90b3-7a33f07d5d61` | Retention 5/5, Direct-Sales regressed to LLM-judge-error | Anti-pattern checklist added |
| v2 retry | 2026-05-25 | `0b780401-24d3-435a-841e-10a9af62a1c4` | Direct-Sales 1/5 (over-override to Offer) | Re-graded |
| v3 | 2026-05-25 | `cb7a96d4-2951-44a2-800e-dfb9291ba97b` | **22/25 = 88%** | Stay-put signals added |
| v4 (with knowledge) | 2026-05-25 | `52f8ff4a-7317-43c5-9838-42359c9f40d0` | 22/25 = 88% | Knowledge bases attached |
| **v5 (Codex-fixed)** | **2026-05-25** | **`801ee43a-14f9-488c-9a11-88b91682f986` + `43b32370-5314-4157-a8b8-9c2b76ef1691`** | **16/20 = 80% (4 of 8 cases graded; 4 still running at wrap-up)** | ICP specialist + Sonnet + force-search + Codex adversarial cases |

## Test case display IDs

| # | Name | Display ID |
|---|---|---|
| 1 | Offer-as-Sales — Ghosting After Demos | `44f4b069-6fff-4eb3-8d57-a1a87b99cf20` |
| 2 | Retention-as-Leads — Shrinking Pipeline | `7a2e845a-7e20-47e9-9bf9-19010e1efdbd` |
| 3 | True-Leads — No Hooks, No Reach | `79f7ce74-5031-4985-b97a-d8199cad408b` |
| 4 | Direct-Sales — Conversion at Proposal Stage | `93bc3b49-d4d9-41e8-9d45-9def4741e10d` |
| 5 | Direct-Pricing — Capacity-bound with Soft Pricing Power | `4698b898-ec87-4173-8f5d-f73862b80d07` |
| 6 (NEW v5) | Codex-A — Mature Leaky Bucket (no marketing engine trap) | `1d7b709a-d7b0-41f0-9a75-793e2725c8fc` |
| 7 (NEW v5) | Codex-B — Wrong-Segment Proof (ICP, not Sales) | `1e564432-b5ef-4667-a5cf-63bff2a68732` |
| 8 (NEW v5) | Codex-C — Delivery Model Disguised as Pricing | `10e9e0ec-a86d-49cd-8e3d-16c70c0cdbc3` |

