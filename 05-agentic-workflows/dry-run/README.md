# Dry Run · Juno AWSpec, executed without Langflow

> This is not a Langflow export. Module 5's optional post-class lab asks you to run the AWSpec through a real Langflow agent, that requires Docker/Python 3.10+ and an OpenAI key entered into a form, neither of which I can or should do on your behalf (API keys never get typed in by me, regardless of provider).
>
> Instead, this folder is the same underlying exercise, proving the AWSpec's logic actually produces the right behaviour, executed directly by reading the transcript, retrieving from the real strategy doc, scoring, and routing exactly as `awspec.md` and `agent-control-panel.md` specify. No external runtime, no API key, no simulated numbers.

**Contents**

- [`transcript.md`](transcript.md) - the sample P0 escalation processed in this run.
- [`output.md`](output.md) - the pipeline executed step by step: retrieval, pillar comparison, Jira cross-check, scoring, confidence check, resulting Insight Card and PRD stub.
- [`diagram.md`](diagram.md) - the same trigger → RAG → reasoning → guardrail → route shape as the Langflow graph, annotated with what actually happened on this run.
- [`notes.md`](notes.md) - reflection: what this run showed against the AWSpec's assumptions, and against the earlier real Lovable prototype test.
