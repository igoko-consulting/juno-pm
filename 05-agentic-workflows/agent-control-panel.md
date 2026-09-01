# Agent Control Panel · Juno

> Module 5 · Agentic Workflows. The operator's control surface for Juno, from the **M5 · Agent Control Panel**. 
# Agent Control Panel · Juno

## Autonomy level

Agent can draft a P0 to P3 priority with citations and a PRD section. Agent CANNOT publish, export, or post that draft anywhere without PM approval.

## Controls

- **Kill switch:** max_steps: 6 (the 5-step pipeline from the AWSpec, plus 1 evidence-balance retry). Abort if the same tool fails twice in a row. Hard timeout: 15s wall clock, 5x the 3s p95 latency target from the PRD, enough margin to not mask a real stall.
- **Rate / cost caps:** ~$0.07 per run at current blended token cost (PRD requirement 1). Cap at 50 runs per day per workspace (~$3.50), alert the PM lead if exceeded, a spike usually means something upstream is misconfigured, not genuine demand.
- **Escalate-on-stuck:** After retrieval returns fewer than 3 relevant segments, or the evidence-balance gate fails twice, degrade to the "insufficient evidence" banner, no priority assigned, PM prompted to load the strategy doc or escalate manually. After 2 tool errors in a run, abort and log the full trace for PM review.

## Monitoring

**Confidence thresholds (map to actions):**

≥ 70% → tag P0/P1, ready for the PM review queue. 30 to 69% → tag P2/P3 "medium confidence," mandatory PM review before it's treated as final. < 30% → notRecommended, no priority assigned, straight to PM judgement.

**Checkpoints:**

Any priority under 70% confidence requires PM review before it counts as final. Any conflicting signal between Jira and Slack (PRD requirement 5) requires PM review, the agent doesn't pick a side. Any transcript Juno can't map to a strategic pillar is tagged "Outside current strategy" and goes straight to PM judgement.

**North Star (re-read every loop):**

You are Juno. Your single goal is to turn each new P0/P1 escalation into a ranked, cited priority for the PM's daily review. Always cite a strategic pillar and a source ID. Never invent customer names, ARR figures, or PII. Mark NEEDS CLARIFICATION instead of guessing. Never publish without PM approval.

## Permissions

READ: Slack #escalations, Notion Strategy One-Pager, Jira ROCKET tickets. WRITE: Juno's own draft store only. CANNOT write to Jira, Slack, Notion, or Salesforce under any circumstance.

## Self-review

- [ ] Stop conditions include max_steps + wall-clock timeout.
- [ ] Tool outputs include a confidence/score field per retrieval tool.
- [ ] Confidence thresholds map to actions, not just labels.
- [ ] North Star is one sentence, re-read every loop.
- [ ] Each rule of engagement names something the agent CANNOT do.
