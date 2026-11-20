# AI Strategy One-Pager - Juno Automated Prioritization

## 1. Problem & Workflow

The Problem: roadmap discussions at RocketShip are driven by the loudest voice in Slack rather than customer evidence. Priorities reverse weekly; stakeholder trust is eroding.

Prevention: Juno explicitly prevents 'opinion-driven prioritization' - the bad decision of moving a feature up the backlog because someone in #leadership posted strongly, instead of because the cited evidence outweighs the alternatives. The good decision it enables: a priority any stakeholder can trace back to its sources and re-litigate on the evidence, not on who posted loudest.

## 2. Target Metrics

Cycle time: reduce average weekly roadmap prioritization from 2 hours to 30 minutes (75% reduction).

Leadership proof: under-10% rate of decisions reversed within 1 week (tracked via Jira priority-change log), AND 90%+ of prioritised items have at least 2 cited sources from the corpus (baseline: 0% today - no citation discipline currently exists). Both metrics measurable in the first 30 days post-launch.

Cost translation (modelled, not measured): at 6 PMs recovering 1.5 hrs/week each (the cycle-time saving above) and a $95/hr loaded PM cost, that's roughly $11,100 recovered per quarter. Modelled build cost is $45,000 (build + initial eval harness), putting payback at approximately 4 quarters. It excludes running costs (PRD requirement 1), the PM time spent reviewing cards and grading evals, and the harder-to-quantify upside of fewer wrong bets, so treat it as a modelled estimate, not a floor.

## 3. Autonomy Level

Choice: Copilot. Juno drafts a P0 to P3 priority with a confidence score and source citations; the PM reviews and clicks 'approve' before anything publishes (PRD requirement 6).

Explicitly avoiding: Agent. Letting Juno move sprint priorities or shift live dates without a human approval step is a one-way trust-erosion door - a single wrong call lets stakeholders dismiss the system permanently. Also passing on pure Reactive/Assist: waiting for a supervisor to ask before drafting anything erases the time savings that justify automating a weekly bottleneck.

## 4. Data & Model Approach

Approach: Hybrid - Ground (RAG) + bounded Agentic cross-check. We ground the model in the RocketShip corpus (Slack #escalations, Notion product pages, Jira tickets and this one-pager) so every priority cites a source ID. A bounded agentic layer cross-checks Jira priority against Slack escalations and flags conflicts; it surfaces discrepancies, it doesn't resolve them.

Explicitly avoiding: a generic LLM (Buy) - without RAG grounding, Juno would hallucinate plausible-sounding priorities and invent customer signals that don't exist, the failure mode that kills trust fastest. Also avoiding Fine-tune: it goes stale as the corpus moves and still can't cite a live source the way RAG retrieval can.

**Strategic pillars.** Every priority is scored against one of these four, this is what "strategic-rationale citation" in the AWSpec actually points to:

1. **Platform Reliability** - uptime and crash-free performance, especially on data-heavy operations (large exports, high-volume queries).
2. **Trust & Security** - authentication, access control, data integrity.
3. **Customer Retention** - reducing churn, weighted toward the quieter SMB signal flagged in Risk 1 below, not just the loudest enterprise escalation.
4. **Operational Efficiency** - reducing manual PM and support toil, the same value frame Juno itself is built to deliver.

## 5. Risks & Mitigations

Risk 1: training data lag. Juno could over-weight whichever signal type was loudest in the past 60 days (e.g. enterprise escalations) and systematically under-weight quieter but more strategic signals (e.g. SMB churn). One quarter of skewed priorities and the roadmap drifts.
Mitigation: a hard 'evidence balance' eval gate that rejects any draft where one source type supplies more than 80% of cited sources. Run weekly; PM reviews.

Risk 2: metric-gaming. Chasing the 90% citation-coverage target could reward Juno for attaching two tangentially related sources just to hit the number, not for genuine evidential strength.
Mitigation: PM spot-checks a 10% sample of citations weekly for relevance, not just presence - coverage is a floor, not a proxy for quality.

## 6. V1 Scope

In: (1) scoring each P0/P1 escalation into a cited priority card, (2) surfacing under-cited items, (3) flagging conflicts between Slack escalations and Jira priorities.

Out: (1) hiring or headcount decisions, (2) customer-facing comms about why a feature was deprioritised. Both stay 100% with the human PM.
