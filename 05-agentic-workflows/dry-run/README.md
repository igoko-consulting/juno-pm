# Dry run · Juno AWSpec, walked through by hand

> This is a walkthrough by hand, not a Langflow run. The AWSpec and Agent Control Panel are applied step by step to one reconstructed escalation: reading the thread, picking the relevant parts of the strategy one-pager, scoring and routing. The Jira ticket is hypothetical and the 82% confidence is a judgement, not a computed score.
>
> It has not been run in Langflow or against a live model. That is Sprint 2 in the README's execution plan.

**Contents**

- [`transcript.md`](transcript.md) - the sample P0 escalation processed in this run.
- [`output.md`](output.md) - the pipeline applied step by step: retrieval, pillar comparison, Jira cross-check, scoring, confidence check, resulting Insight Card and PRD stub.
- [`diagram.md`](diagram.md) - the same trigger → RAG → reasoning → guardrail → route shape as the Langflow graph, annotated with what actually happened on this run.
- [`notes.md`](notes.md) - reflection: what this run showed against the AWSpec's assumptions, and against the earlier real Lovable prototype test.
