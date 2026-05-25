# Agent IDs — Hormozi GTM Strategist Workforce

Created 2026-05-25. Region `f1db6c`, project `f60582d1-7e8c-4875-9e3f-2082b1d51e74` (Immutable).

## Live agents (v4 / v5 build)

| Role | Agent ID | Model | Notes | App URL |
|---|---|---|---|---|
| Triage | `dcf970a1-9100-42d0-8f7a-7174d5111bcd` | anthropic-claude-sonnet-4-6 | Upgraded v4 (Codex feedback) | [open](https://app.relevanceai.com/agents/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/dcf970a1-9100-42d0-8f7a-7174d5111bcd/edit/instructions) |
| ICP / Positioning | `5f3ffebf-8e8f-416c-8968-18f12a9347a9` | relevance-cost-optimized | Added v4 (Codex feedback — missing root node) | [open](https://app.relevanceai.com/agents/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/5f3ffebf-8e8f-416c-8968-18f12a9347a9/edit/instructions) |
| Offer | `f4bbb2ec-fb0b-4625-8601-9094824a0ff0` | relevance-cost-optimized | Force-search added v4 | [open](https://app.relevanceai.com/agents/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/f4bbb2ec-fb0b-4625-8601-9094824a0ff0/edit/instructions) |
| Leads | `5de5f58e-36a6-4867-b3e5-832f651ea8ed` | relevance-cost-optimized | Force-search added v4 | [open](https://app.relevanceai.com/agents/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/5de5f58e-36a6-4867-b3e5-832f651ea8ed/edit/instructions) |
| Sales | `1e249cd1-e210-473f-983e-bcdf5285995c` | relevance-cost-optimized | Force-search added v4 | [open](https://app.relevanceai.com/agents/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/1e249cd1-e210-473f-983e-bcdf5285995c/edit/instructions) |
| Retention | `7776cf4d-6f52-4b17-bbe1-e6671a8f3328` | relevance-cost-optimized | Force-search added v4 | [open](https://app.relevanceai.com/agents/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/7776cf4d-6f52-4b17-bbe1-e6671a8f3328/edit/instructions) |
| Pricing | `1026f7bc-73fc-4bfc-a9ae-bd5971b7bd30` | relevance-cost-optimized | Force-search added v4 | [open](https://app.relevanceai.com/agents/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/1026f7bc-73fc-4bfc-a9ae-bd5971b7bd30/edit/instructions) |
| Synthesizer | `54dd1f3b-691e-447a-8b13-4eb310bf7eef` | relevance-cost-optimized | Unchanged from v3 | [open](https://app.relevanceai.com/agents/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/54dd1f3b-691e-447a-8b13-4eb310bf7eef/edit/instructions) |

## Knowledge sets (v4)

| Domain | Knowledge set ID | Row count | Attached to |
|---|---|---|---|
| Offer | `hormozi-offer-source` | 8 | Offer Specialist |
| Leads + Advertising | `hormozi-leads-source` | 24 | Leads Specialist |
| Sales | `hormozi-sales-source` | 14 | Sales Specialist |
| Retention + Nurture | `hormozi-retention-source` | 23 | Retention Specialist |
| Pricing | `hormozi-pricing-source` | 11 | Pricing Specialist |
| ICP / Positioning | `hormozi-icp-source` | 5 | ICP Specialist (added v4) |

All knowledge sets attached as `usage_type: tool` (search on demand). Specialists are instructed to ALWAYS run at least one search before producing a diagnosis (v4 addition).

## Autonomy + behaviour

All agents on `autonomy_limit: 5`, `autonomy_limit_behaviour: "terminate-conversation"` (bumped from 3 in v4 after Codex flagged headroom risk for Triage's route + execute + quote-back sequence).

## Workforce

**Workforce ID:** `0bae6bf7-18ca-48ce-a69b-548720551fa0`
**Type:** `default`
**Topology:** Hub-and-spoke. Trigger → Triage → (1 of 6 specialists via tool-call, always-create-new threading) → Triage quote-back → Synthesizer (forced-handover, always-same threading).
**URL:** https://app.relevanceai.com/workforce/f1db6c/f60582d1-7e8c-4875-9e3f-2082b1d51e74/0bae6bf7-18ca-48ce-a69b-548720551fa0/build
