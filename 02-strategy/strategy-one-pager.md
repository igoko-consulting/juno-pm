# AI Strategy One-Pager - Juno Automated Prioritization

## 1. Problem & Workflow

The Problem: roadmap discussions at RocketShip are driven by the loudest voice in Slack rather than customer evidence. Priorities reverse weekly; stakeholder trust is eroding.

Prevention: Juno explicitly prevents 'opinion-driven prioritization' - the bad decision of moving a feature up the backlog because someone in #leadership posted strongly, instead of because the cited evidence outweighs the alternatives. The good decision it enables: a ranking any stakeholder can trace back to its sources and re-litigate on the evidence, not on who posted loudest.

## 2. Target Metrics

Cycle time: reduce average weekly roadmap prioritization from 2 hours to 30 minutes (75% reduction).

Leadership proof: under-10% rate of decisions reversed within 1 week (tracked via Jira priority-change log), AND 90%+ of prioritised items have at least 2 cited sources from the corpus (baseline: 0% today - no citation discipline currently exists). Both metrics measurable in the first 30 days post-launch.

## 3. Autonomy Level

Choice: Copilot. The AI reviews the evidence file and drafts the decision letter with a recommended payout; a human supervisor must click 'approve' before any transaction over $500 goes out. The threshold isn't arbitrary - it calibrates autonomy to blast radius, letting routine, low-stakes claims move faster while anything material still gets a human check.

Explicitly avoiding: Agent. We will not give the AI end-to-end autonomy to move money out of a user's account unsupervised - a single misjudged damage assessment, if it reaches the payout stage before anyone catches it, is a trust-erosion event we can't undo. Also passing on pure Reactive/Assist: waiting for a supervisor to ask before drafting anything erases the time savings that justify automating claims review in the first place.

## 4. Data & Model Approach

Approach: Hybrid - Ground (RAG) + bounded Agentic cross-check. We ground the model in the RocketShip corpus - Slack #escalations, support tickets, interview notes, Notion product pages, Jira tickets - so every priority cites a source ID. A bounded agentic layer cross-checks Jira priority against Slack escalations and flags conflicts; it surfaces discrepancies, it doesn't resolve them.

Explicitly avoiding: a generic LLM (Buy) - without RAG grounding, Juno would hallucinate plausible-sounding priorities and invent customer signals that don't exist, the failure mode that kills trust fastest. Also avoiding Fine-tune: it goes stale as the corpus moves and still can't cite a live source the way RAG retrieval can.

## 5. Risks & Mitigations

Risk 1: training data lag. Juno could over-weight whichever signal type was loudest in the past 60 days (e.g. enterprise escalations) and systematically under-weight quieter but more strategic signals (e.g. SMB churn). One quarter of skewed priorities and the roadmap drifts.
Mitigation: a hard 'evidence balance' eval gate - reject any priority list where less than 20% of cited sources come from any one source type. Run weekly; PM reviews.

Risk 2: metric-gaming. Chasing the 90% citation-coverage target could reward Juno for attaching two tangentially related sources just to hit the number, not for genuine evidential strength.
Mitigation: PM spot-checks a 10% sample of citations weekly for relevance, not just presence - coverage is a floor, not a proxy for quality.

## 6. V1 Scope

In: (1) ranking the existing backlog with cited evidence, (2) surfacing under-cited items, (3) flagging conflicts between Slack escalations and Jira priorities.

Out: (1) hiring or headcount decisions, (2) customer-facing comms about why a feature was deprioritised. Both stay 100% with the human PM.
