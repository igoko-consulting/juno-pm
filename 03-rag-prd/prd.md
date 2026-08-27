# AI PRD · Juno

_Module 3.

## Problem & user

RocketShip PMs need evidence-based prioritisation. Today roadmap calls follow whoever posts loudest in Slack, not the customer evidence, and priorities reverse weekly as a result. Juno turns noisy Slack, Jira, and strategy docs into a ranked, cited backlog a PM can defend without having to win an argument on volume.

## Solution overview

Juno is a hybrid system: RAG for grounded retrieval, plus a bounded agentic layer that cross-checks Jira priority against Slack escalations and flags where they disagree. The agentic layer surfaces conflicts, it does not resolve them, and it never publishes anything on its own. Every output goes to the PM for approval first (see Autonomy, below).

**Retrieval strategy:** Hybrid

PMs ask two kinds of questions. Specific: "What did the Pearson Co ticket from 14 October cite as the blocker?" needs keyword retrieval. Directional: "What's the dominant frustration theme this week?" needs semantic. RAG-only sacrifices the second; long-context-only sacrifices precision and blows the budget on the first.

**Autonomy:** Copilot, not Agent. Juno drafts the ranked backlog with citations. A PM has to click approve before anything publishes. This matches the autonomy decision already made in the Strategy One-Pager, and it's a requirement below, not a footnote.

## Retrieval requirements (RAG)

- **Sources:** RocketShip Strategy One-Pager (M2 deliverable), Slack `#escalations` tagged P0/P1, Notion pages in the RocketShip Product workspace, Jira tickets in the `ROCKET` project. This matches the scope already set in Juno's system prompt and the One-Pager. Earlier drafts of this PRD referenced Zendesk, Salesforce, and a `#voice-of-customer` channel; none of those are in scope and they've been removed rather than left as an open inconsistency.
- **Volume:** roughly 600 documents to start. Worth re-checking this figure once the corpus is actually indexed against the corrected source list above, since it was originally estimated against a different mix.
- **Chunking / indexing:** hybrid, semantic and keyword. Around 500-token chunks with 15% overlap feed the semantic index for directional queries. A keyword (BM25) index over ticket IDs, Slack permalinks, and Notion page IDs handles the specific lookups. This maps directly to the two query types above.
- **Grounding rule:** every priority Juno produces (P0 to P3, or notRecommended) cites at least one strategy clause and at least one piece of evidence (Jira key, Slack permalink, or Notion page reference). Citations render inline as footnotes the PM can click through to verify.
- **Freshness:** Strategy One-Pager syncs on commit, since it lives in git. Slack `#escalations` refreshes hourly, since that's where the fastest-moving signal is. Jira tickets refresh hourly too, since status and priority changes are exactly what the agentic layer is checking for conflicts against. Notion pages sync daily, since product docs move slower than either.
- **Staleness disclosure:** if any source is past its freshness window when a citation is generated (for example, a sync job failed), the citation shows an "as of" timestamp rather than presenting the data as current. Silent staleness undermines the whole point of the grounding rule.
- **Citation integrity:** at generation time, Juno checks that a cited source still exists and hasn't materially changed since it was indexed. If a Slack message was deleted or a Jira ticket was edited after indexing, the citation is marked unverified instead of being shown as valid.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Retrieval quality and latency | Must | Top-K = 8 retrieval segments per prioritisation run. p95 latency target under 3 seconds end to end, from clicking Process to a ranked draft. At our $0.03 per 1k token blended cost, this runs about $0.07 per Juno run, which is fine for daily PM use. Re-benchmark this target once the corpus passes roughly 1,200 documents (2x the starting volume), since retrieval time and cost both scale with index size. |
| 2 | Fail-safe on empty retrieval | Must | If retrieval returns fewer than 3 relevant segments, or no strategy doc is loaded, Juno does not produce a P0/P1 ranking. It returns a banner instead: "Insufficient evidence to recommend priority, load a strategy document or escalate to PM judgement." This is a feature, not a failure. |
| 3 | Grounded trust | Must | Every priority cites at least one strategy clause and at least one piece of evidence, rendered as clickable inline footnotes. |
| 4 | Evidence-balance gate | Must | Reject any priority list where fewer than 20% of cited sources come from any single source type (per the Strategy One-Pager's risk mitigation). The gate runs before the draft reaches the PM. On failure, retrieval re-runs once with rebalanced source weighting. If the second attempt still fails the threshold, Juno falls back to the same insufficient-evidence banner from requirement 2, rather than looping indefinitely or shipping a skewed list anyway. |
| 5 | Conflict flagging | Must | When the agentic layer finds a Jira priority that contradicts an open Slack escalation (for example, a P2 ticket tied to a thread tagged P0), Juno surfaces both citations side by side in the draft with a "conflicting signal" label. It does not pick a side. |
| 6 | Approval gate | Must | No prioritised backlog or PRD draft is published, shared, or exported until a PM clicks approve. This is the Copilot autonomy boundary and it's enforced in the UI, not just stated as policy. |

## Out of scope

- Decisions that cannot be cited to a source in the knowledge base.
- Hiring or headcount decisions.
- Customer-facing communications about why a feature was deprioritised.
- Any autonomous action beyond drafting. Requirement 6 covers the approval gate; this line just confirms nothing here changes that.
