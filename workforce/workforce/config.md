# Workforce Configuration: Hormozi GTM Strategist

## Architecture

**Pattern:** Hub-and-Spoke (Triage Agent as orchestrator, 5 specialists as tool-callable agents, Synthesizer in line)

**Topology:**

```
                   ┌──────────────────────┐
                   │  Manual Trigger      │
                   │  (user chat input)   │
                   └──────────┬───────────┘
                              │
                   ┌──────────▼───────────┐
                   │  TRIAGE AGENT        │ ◄────── decides which
                   │  (hub)               │         specialist to call
                   └──────────┬───────────┘
                              │
            ┌─────┬───────────┼───────────┬─────────┐
        tool-call edges (always-create-new threading)
            │     │           │           │         │
            ▼     ▼           ▼           ▼         ▼
         OFFER  LEADS       SALES     RETENTION  PRICING
                            (specialists, no tools)
                              │
                   ┌──────────▼───────────┐
                   │  Specialist returns  │
                   │  brief to Triage     │
                   └──────────┬───────────┘
                              │ Triage's final response includes the specialist's brief
                              │ forced-handover (always-same threading)
                   ┌──────────▼───────────┐
                   │  SYNTHESIZER         │ ◄────── wraps with cross-level
                   │                      │         stress tests
                   └──────────┬───────────┘
                              │
                   ┌──────────▼───────────┐
                   │  Final GTM Brief     │
                   │  to user             │
                   └──────────────────────┘
```

## Why hub-and-spoke (not linear, not condition node)

- **Linear chain (default):** Would run all 5 specialists every time. Wasteful, contradicts the Constraint Hierarchy (only one specialist should engage per problem), and dilutes the demo narrative.
- **Condition node:** Could work but adds an extra agent in the chain just for routing logic. Triage is already doing diagnostic reasoning — adding a separate condition node duplicates that. Hub-and-spoke lets Triage's reasoning drive routing directly.
- **Hub-and-spoke:** Triage decides which specialist to invoke based on its diagnosis. The specialist runs in isolation. Triage receives the specialist's response text and passes it to Synthesizer via forced-handover. Best fits the architectural intent.

## Workforce Graph (Relevance AI nodes + edges)

### Nodes

| node_id | type | config notes |
|---|---|---|
| `trigger-manual` | trigger | `{ type: "manual" }` |
| `agent-triage` | agent | entity_link → Triage agent_id, project, region |
| `agent-offer` | agent | entity_link → Offer specialist agent_id |
| `agent-leads` | agent | entity_link → Leads specialist agent_id |
| `agent-sales` | agent | entity_link → Sales specialist agent_id |
| `agent-retention` | agent | entity_link → Retention specialist agent_id |
| `agent-pricing` | agent | entity_link → Pricing specialist agent_id |
| `agent-synthesizer` | agent | entity_link → Synthesizer agent_id |

### Edges

| edge_id | source → target | edge_type | threading | notes |
|---|---|---|---|---|
| `e1-trigger-to-triage` | trigger-manual → agent-triage | forced-handover | always-same | User's input flows to Triage |
| `e2-triage-to-offer` | agent-triage → agent-offer | **tool-call** | always-create-new | Triage decides whether to invoke. Specialist returns response text only. |
| `e3-triage-to-leads` | agent-triage → agent-leads | **tool-call** | always-create-new | same |
| `e4-triage-to-sales` | agent-triage → agent-sales | **tool-call** | always-create-new | same |
| `e5-triage-to-retention` | agent-triage → agent-retention | **tool-call** | always-create-new | same |
| `e6-triage-to-pricing` | agent-triage → agent-pricing | **tool-call** | always-create-new | same |
| `e7-triage-to-synthesizer` | agent-triage → agent-synthesizer | forced-handover | always-same | After specialist returns, Triage hands off to Synthesizer |

### Action config (for tool-call edges)

Each specialist tool-call edge needs:

```json
{
  "edge_type": "tool-call",
  "config": {
    "threading_behavior": { "type": "always-create-new" },
    "action_config": {
      "action_behaviour": "never-ask",
      "wait_for_completion": true,
      "prompt_for_when_to_use": "<see specialist-specific prompts below>",
      "params_schema": {
        "properties": {
          "specialist_brief": {
            "type": "string",
            "description": "The structured brief for the specialist, including: the situation, the framework to apply, what to produce, and the cross-level constraint to preserve."
          }
        },
        "required": ["specialist_brief"]
      }
    }
  }
}
```

### `prompt_for_when_to_use` per specialist (injected into Triage's system prompt)

- **Offer specialist:** "Use this when the diagnosis is an Offer constraint — Value Equation imbalance, weak Perceived Likelihood, undifferentiated value proposition, prospects loving the product on demo but ghosting after."
- **Leads specialist:** "Use this when the diagnosis is a Leads constraint — pipeline too small, channel imbalance, weak hooks, no lead magnet pulling, top-of-funnel not generating qualified opportunities."
- **Sales specialist:** "Use this when the diagnosis is a Sales constraint — conversations breaking at a specific CLOSER step, objections unhandled, deals stalling at proposal, can't reproduce wins."
- **Retention specialist:** "Use this when the diagnosis is a Retention constraint — high churn, low expansion, no referrals, customers stop using the product, NPS dropping."
- **Pricing specialist:** "Use this when the diagnosis is a Pricing constraint — at capacity but margins flat, no annual option, no premium tier, competing on price, value-capture failure on a known-good offer."

## Workforce metadata

```json
{
  "workforce_metadata": {
    "name": "Hormozi GTM Strategist",
    "type": "default",
    "description": "A 7-agent workforce that diagnoses B2B SaaS GTM problems using Alex Hormozi's Constraint Hierarchy. Triage routes to one specialist (Offer, Leads, Sales, Retention, Pricing); Synthesizer wraps with cross-level stress tests."
  }
}
```

## Build sequence (when MCP is available)

1. Create all 7 agents individually (`relevance_create_agent`) — saves each as draft. Capture all 7 agent_ids and the project/region values from each response.
2. For each agent, call `relevance_publish_agent` to promote draft to active. **Workforce nodes require published `active_version_id`, not draft ids.** Verify with `relevance_get_agent` (default returns draft; use `version: "active"` to confirm the live version exists).
3. Create the workforce with all 7 nodes and the 7 edges via `relevance_create_workforce`. Pass the published agent_ids in.
4. Test with the demo input (`relevance_trigger_workforce`).
5. Inspect execution with `relevance_get_workforce_task_messages` — verify routing decision, specialist output, synthesizer wrap.
6. Iterate Triage prompt if any eval test case misroutes. Remember: edits go to draft only — re-publish after each iteration.

**Time budget (MCP path):** ~30 min for agent creation + publishing + workforce wiring, plus eval iteration time.

## Manual UI build (if MCP unavailable)

1. Go to https://app.relevanceai.com → Agents → Create.
2. For each of the 7 agents in `/agents/`, paste the system prompt and metadata. Save as draft. **Then publish** (workforce nodes need published versions).
3. Go to Workforces → Create. Add all 7 agents as nodes. Wire the 7 edges using the configuration above.
4. For each tool-call edge (Triage → specialist), fill in the action_config in the edge config panel: `params_schema`, `prompt_for_when_to_use`, `action_behaviour: never-ask`, `threading_behavior: always-create-new`. **This is ~5 minutes per edge × 5 edges = ~25 minutes of UI clicking.** Budget for this.
5. Test from the workforce trigger page with the demo input.

**Time budget (UI path):** ~60-75 min total. Slower than MCP but more reliable when the plugin is intermittent.
