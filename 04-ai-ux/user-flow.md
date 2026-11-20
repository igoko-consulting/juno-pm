# AI-Native User Flow · Juno

_Module 4._

## Entry point

**Signal type:** New #escalations thread received

A new #escalations thread tagged P0 arrives in the Raw Input column in Juno (the dashboard built in M1) through a Slack webhook. In the prototype, pasting the thread as a transcript stands in for the webhook. There is no "Start AI" click: the arrival itself is the trigger.

**What they see instantly**

"Juno is reading your transcript..." status pill appears under the input column. The empty Insights and PRD columns dim to roughly 40% to signal the agent is working, so the PM never wonders if anything happened.

## The flow

**Visible to the PM**

1. Status pill fires the moment the transcript lands, no manual trigger.
2. Breadcrumb sequence: "Scanning Strategy One-Pager..." then "Cross-referencing transcript with strategic pillars..." then "Synthesising priorities and drafting PRD section..." These three messages are paced against the roughly 3 second p95 latency target from the M3 PRD, so the wait reads as progress rather than a stall.
3. Path A, strategy loaded: grounded prioritisation with citations, rendered as Insight Cards.
   Path B, strategy missing: no priority. Juno shows the insufficient-evidence banner and a prompt to load the strategy doc (PRD requirement 2).
4. PM reviews the cards and PRD draft, edits where needed, and clicks Approve. Nothing publishes or exports before this. This is the Copilot boundary from the Strategy One-Pager and PRD requirement 6, and the point where the PM takes ownership.

**Hidden logic** (not shown to the PM, referenced here so it stays traceable to the M3 PRD)

1. RAG retrieval over the RocketShip Strategy One-Pager, top-K = 6. This is lower than the PRD's top-K = 8 because the lookup covers one short document with four pillars, not the 600-document corpus.
2. Comparison logic: does the transcript's pain point map to a strategic pillar?
3. Risk and alignment scoring: emit P0 to P3 with a strategic-rationale citation.
4. Confidence check: confidence sets routing, not priority. 70 or above goes to the PM review queue as high confidence. 30 to 69 carries a "medium confidence" flag and needs PM review before it counts as final. Under 30 returns notRecommended with no priority.

## AI moments

**Placement:** Inline and embedded

One Insight Card per escalation in column 2, with a P0 to P3 badge, an evidence quote from the thread, and a Strategic Traceability footer citing the pillar. The PRD draft in column 3 references the card directly.

Two further card states come from the PRD:

- **Evidence-balance flag:** if the evidence-balance gate (PRD requirement 4) rejects a draft because one source type supplies more than 80% of its citations, the card shows a "rebalancing evidence" state rather than the PM seeing nothing change.
- **Conflicting signal flag:** if the agentic layer finds a Jira priority that contradicts an open Slack escalation (PRD requirement 5), the card shows both citations side by side with a "conflicting signal" label. Juno doesn't pick a side, the PM does.

Value prop stays augmentation (M2). The PM is the validator, editing and approving rather than creating from a blank page. Keeping this inline, next to the raw transcript on the left, keeps them in flow instead of pulling their attention to a separate panel.

## Fallbacks

**Kill switch**

Every Insight Card has a Manual Override button: re-tag P0 to P3, edit the rationale, or mark notRecommended. PRD blocks are individually editable and regenerable.

**Training signal**

A manual demote logs as a "strategic-alignment correction." Three or more similar overrides flags the strategic pillar as ambiguous and tightens retrieval against it.

**Fail-safe**

If RAG returns no match for a transcript's pain point, Juno tags the card "Outside current strategy" with an amber warning. It never invents an alignment. The PM can promote it manually or send it back for clarification.

If the evidence-balance gate fails twice in a row (see PRD requirement 4), Juno falls back to the same insufficient-evidence banner used when retrieval comes back empty, rather than shipping a skewed card or retrying indefinitely.

## Self-review

- [x] Trigger fires on the earliest possible signal, no manual "Start AI" click.
- [x] At least one breadcrumb message turns latency into transparency.
- [x] The AI's role matches the M2 value proposition (Automation / Augmentation / Insights / Personalisation).
- [x] Every automated decision has a working kill switch.
- [x] Fail-safe path is explicit, no dead end on a bad AI result.
- [x] Hidden logic references M3 PRD specs (Top-K, latency target, knowledge base).
- [x] Approval gate is a visible, blocking step before anything publishes.
