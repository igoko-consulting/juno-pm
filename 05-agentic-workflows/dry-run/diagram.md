# Dry run diagram · THREAD-2201

Same shape as the Langflow graph in `Juno Agent.json`, annotated with what actually happened on this run rather than placeholder boxes.

```mermaid
flowchart TD
    trigger["Trigger\nNew P0 thread: THREAD-2201"]
    retrieve["RAG retrieval, top-K=6\nover Strategy One-Pager"]
    compare["Compare to 4 pillars\nMatch: Platform Reliability"]
    crosscheck["Cross-check Jira\nROCKET-1188 = P2 vs Slack = P0"]
    score["Score risk + alignment\nConfidence: 82%"]
    guardrail{"Confidence >= 70%\nAND no conflict?"}
    queue["Route: PM review queue\n(high confidence)"]
    conflict["Flag: conflicting signal\nforces PM review regardless"]
    draft["Draft Insight Card + PRD stub\njuno.draft_card, juno.draft_prd_section"]
    approve["PM approval gate\nnothing publishes without it"]

    trigger --> retrieve --> compare --> crosscheck --> score --> guardrail
    guardrail -- "82% confidence" --> queue
    guardrail -- "Jira/Slack conflict" --> conflict
    queue --> draft
    conflict --> draft
    draft --> approve
```

**What this run actually exercised:** the AWSpec's Checkpoints rule says a conflicting signal forces PM review "regardless" of confidence. This is the first time that specific branch has been run against a real example, every prior document described it, none had shown a case where high confidence and a conflict happen on the same card. Here confidence was high (82%) and the conflict flag still won, PM review is required either way.
