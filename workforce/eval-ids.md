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

Each test case has 5 LLM-judge checks. 25 rules total. Target: 22/25 (88%) before Loom recording.

## URLs

Workforce: https://app.relevanceai.com/workforce/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/0bae6bf7-18ca-48ce-a69b-548720551fa0/build

## Final eval results (v3, after Triage prompt iteration)

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
- **v3 (override + stay-put signals): `f6551579-4e8c-4f87-bc5b-8ea09a192a5c`** ← ACTIVE

## Knowledge bases (added post-v3)

Each specialist now has its domain source material attached as a `tool` knowledge set. Agents can semantically search and pull verbatim Hormozi passages on demand.

| Specialist | Knowledge Set ID | Rows | Source files |
|---|---|---|---|
| Offer | `hormozi-offer-source` | 8 | `offers.md` |
| Leads | `hormozi-leads-source` | 24 | `leads.md`, `advertising.md` |
| Sales | `hormozi-sales-source` | 14 | `sales.md` |
| Retention | `hormozi-retention-source` | 23 | `retention-and-proof.md`, `nurture-and-branding.md` |
| Pricing | `hormozi-pricing-source` | 11 | `pricing.md` |

Total: ~88 rows of source-text passages (~176K chars). Embedded with `openai/text-embedding-3-large`.

**Eval v4 (with knowledge bases attached): 22/25 = 88%** — held the target with no regression. Routing all 5 cases correctly. The 3 cross-level-stress-test misses are stylistic (Synthesizer phrasing varies between runs).

