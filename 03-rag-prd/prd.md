# AI PRD · Juno

_Module 3. 

## Problem & user

RocketShip PMs need evidence-based prioritization. Juno turns noisy Slack, tickets, and strategy docs into a ranked, cited backlog they can defend.

## Solution overview

**Retrieval strategy:** Hybrid

PMs ask both kinds of questions. Specific: "What did the Pearson Co Oct 14 ticket cite as blocker?" - needs keyword retrieval. Vibes: "What is the dominant frustration theme this week?" - needs semantic. Going RAG-only sacrifices narrative; long-context only sacrifices precision and budget.

## Retrieval requirements (RAG)

- **Sources:** RocketShip Strategy One-Pager (M2 deliverable) + last 90 days of Slack #voice-of-customer + Zendesk tickets tagged P0/P1 + Salesforce closed-lost notes.
- **Volume:** ~600 documents total. Strategy doc is the one true authority - all other sources are evidence in support.
- **Chunking / indexing:** Hybrid (semantic + keyword). ~500-token chunks with 15% overlap feed the semantic index for "vibes" queries; a keyword (BM25) index over ticket IDs, deal IDs, and Slack permalinks handles the "specific" lookups - directly matching the two query types above.
- **Grounding rule:** Every priority Juno produces (P0-P3 or notRecommended) cites at least one strategy clause AND at least one piece of evidence (ticket ID, Slack permalink, deal note ID). The PRD draft renders citations inline as footnotes the PM can click to verify.
- **Freshness:** Strategy One-Pager: sync on commit (it lives in the team Notion / git). Slack + Zendesk: refresh hourly to keep recent customer signal current. Salesforce: daily sync at 02:00 UTC - lost-deal notes are a leading indicator, not a real-time channel.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Retrieval quality and latency | Must | Top-K = 8 retrieval segments per prioritization run. p95 latency target < 3s end-to-end (from "Process" click to ranked PRD draft). At our $0.03/1k token blended cost, this lands at ~$0.07 per Juno run - acceptable for daily PM use. |
| 2 | Fail-safe on empty retrieval | Must | If retrieval returns < 3 relevant segments OR if no strategy doc is loaded, Juno does NOT produce a P0/P1 ranking. Instead it returns a clear banner: "Insufficient evidence to recommend priority - load a strategy document or escalate to PM judgement." This is a feature, not a failure. |
| 3 | Grounded trust | Must | Every priority Juno produces (P0-P3 or notRecommended) cites at least one strategy clause AND at least one piece of evidence (ticket ID, Slack permalink, deal note ID). The PRD draft renders citations inline as footnotes the PM can click to verify. |
| 4 | Evidence-balance gate | Must | Reject any priority list where fewer than 20% of cited sources come from any single source type (per Strategy One-Pager Risk 1 mitigation). Gate runs automatically before the draft reaches the PM; a failed run re-triggers retrieval with rebalanced source weighting rather than shipping a skewed list. |

## Out of scope

- Decisions that cannot be cited to a source in the knowledge base.
- Hiring or headcount decisions.
- Customer-facing communications about why a feature was deprioritised.
- Publishing without human sign-off: Juno drafts, the PM approves (Copilot autonomy, not Agent - see Strategy One-Pager §3). Anything Juno is not allowed to retrieve or act on without a human falls here.
