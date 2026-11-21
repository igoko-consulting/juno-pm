# Juno PM

Juno is a copilot that turns noisy Slack, Jira and Notion escalations into a cited priority card, which a PM approves before anything is shared. RocketShip, the company it is specified for, is a fictional case study adapted from Product School's AI Product Management course, whose modules the chapters here draw on.

**AI approach:** hybrid keyword and semantic retrieval grounds each card in its sources, a read-only agent step flags Jira/Slack priority conflicts without resolving them, and a PM approval gate blocks anything from publishing.

**Status:** specified end to end and walked through by hand on one reconstructed escalation. Not yet run on a live model, and the golden set is not built. In the one before-and-after test, RAG reworded the priorities but did not change the ranking ([notes](03-rag-prd/juno-rag-diff.md)).

**Start here:** [Strategy one-pager](02-strategy/strategy-one-pager.md) · [AI PRD](03-rag-prd/prd.md) · [Eval stack](06-evals/eval-stack.md)

---

## PM Execution Plan

### Where Juno is today
- All six modules drafted (M1 to M6) and cross-referenced.
- A walkthrough by hand of one reconstructed escalation shows the spec routes a Jira/Slack conflict to PM review even at high confidence.
- Not yet running against a live model or in production, and the golden set is not built.
- Modelled payback of about four quarters on PM time saved (Strategy One-Pager §2). It excludes running costs, PM review and grading time, and the upside of fewer wrong bets.

### What ships next (two sprints)
- **Sprint 1:** build the 100-item golden set and wire the code-based CI checks (format, citation, evidence balance). This closes the biggest gap: accuracy has been shown on one worked case, not at scale.
- **Sprint 2:** rebuild `Juno Agent.json` in Langflow and run it against a live model to measure model variance, latency and tool-call failures, which the golden set can't test.

### What I watch
- **Daily:** Manual Override rate, time to approve, un-actioned rate, and cost per run against the 50-run daily cap (modelled at $6 or more a day).
- **Weekly:** decision-reversal rate, human-rubric mean per dimension, evidence-balance gate trigger rate.
- **Per release** (CI gate and nightly run): golden-set accuracy, and pass rates for the format, citation and evidence-balance checks.

### Red lines (what blocks shipping)
- Golden-set accuracy below 90% on the automated layer.
- Any fabricated PII, ARR or customer name.
- Any critical safety fail (a score of 1) on the human-eval Safety/refusal dimension.
- Any citation-check failure.
- Anything published without a PM approval click.

All five are automatic blocking gates in `06-evals/eval-stack.md`; none is left to PM discretion.

### Governance
- **Compliance:** no external comms and no publishing without human review (system prompt Refusal Conditions, PRD requirement 6).
- **Safety:** hard refusal on PII, ARR, contracts and legal or regulator requests, routed to the human PM.
- **Reliability:** the evidence-balance gate blocks any card where one source type supplies more than 80% of citations; p95 latency target under 3 s (PRD requirement 1).
- **Reputation:** `04-ai-ux/trust-gaps.md` tackles hallucination, opacity and lack of control directly, rather than assuming citations and an approval button are enough.

---

## Module artefacts

_The diagrams were exported from an earlier draft. Where they differ from the text, the text is current._

### M1 · Prompting
- **System prompt:** [`01-prompting/system-prompt.md`](01-prompting/system-prompt.md)
- **Lovable prototype:** [`01-prompting/lovable-prototype.md`](01-prompting/lovable-prototype.md), [prototype link](https://id-preview--a7059b7a-c63b-49a5-92ea-a899d5941eb0.lovable.app/) (private Lovable preview, not publicly viewable)

### M2 · Strategy
- **Decision matrix:** [`02-strategy/decision-matrix.md`](02-strategy/decision-matrix.md)
- **AI strategy one-pager:** [`02-strategy/strategy-one-pager.md`](02-strategy/strategy-one-pager.md)

### M3 · RAG / AI PRD
- **AI PRD:** [`03-rag-prd/prd.md`](03-rag-prd/prd.md)
- **RAG before-and-after test (working notes):** [`03-rag-prd/juno-rag-diff.md`](03-rag-prd/juno-rag-diff.md)

### M4 · AI-Native UX
- **AI user flow:** [`04-ai-ux/user-flow.md`](04-ai-ux/user-flow.md) ([diagram](04-ai-ux/user-flow.png))
- **Trust-gap mitigations:** [`04-ai-ux/trust-gaps.md`](04-ai-ux/trust-gaps.md)

### M5 · Agentic Workflows
- **Agent Workflow Spec (AWSpec):** [`05-agentic-workflows/awspec.md`](05-agentic-workflows/awspec.md) ([diagram](05-agentic-workflows/awspec.png))
- **Agent Control Panel:** [`05-agentic-workflows/agent-control-panel.md`](05-agentic-workflows/agent-control-panel.md) ([diagram](05-agentic-workflows/agent-control-panel.png))
- **Agent graph** (illustrative, not import-tested): [`05-agentic-workflows/Juno Agent.json`](<05-agentic-workflows/Juno Agent.json>)
- **Dry run** (the AWSpec applied by hand to one reconstructed escalation, not run on a live model): [`05-agentic-workflows/dry-run/`](05-agentic-workflows/dry-run/)

### M6 · Evals & Guardrails
- **Eval stack:** [`06-evals/eval-stack.md`](06-evals/eval-stack.md) ([diagram](06-evals/eval-stack.png))
- **Human evaluation rubric:** [`06-evals/human-rubric.md`](06-evals/human-rubric.md) ([screenshot of the rubric builder](06-evals/human-rubric.png))

---

## Build Insights

- **Friction point.** The one before-and-after RAG test reworded the priorities but did not change the ranking: same three issues, same order. Adding the strategy one-pager made the problem statements sharper, so whether grounding changes what gets prioritised is still to be shown on the golden set.
- **Key learning.** A repo-wide consistency check caught something no single-module review would have. Module 1's system prompt still described a ranked top-5 table, while every later module used a single scored priority card. Each module was fine on its own; the set wasn't consistent with itself.
- **Aha moment.** Reading the AWSpec is not the same as running it. The Jira/Slack conflict-check branch was described in three documents but never exercised until the dry run produced a card with both high confidence and a conflict. The conflict flag correctly overrode the confidence score, not the other way round.

---

## Repo structure

```
juno-pm/
├── README.md                    ← this page
├── pitch.html                   ← visual version of this page (GitHub shows the source; download to view)
├── 01-prompting/
│   ├── system-prompt.md         ← M1: Juno's system prompt
│   └── lovable-prototype.md     ← M1: prototype link + debrief
├── 02-strategy/
│   ├── decision-matrix.md       ← M2: build / buy / fine-tune call
│   └── strategy-one-pager.md    ← M2: AI strategy one-pager
├── 03-rag-prd/
│   ├── prd.md                   ← M3: AI PRD with retrieval requirements
│   └── juno-rag-diff.md         ← M3: RAG before-and-after notes
├── 04-ai-ux/
│   ├── user-flow.md             ← M4: AI-native user flow
│   ├── user-flow.png            ← M4: flow diagram (earlier draft)
│   └── trust-gaps.md            ← M4: trust-gap mitigations
├── 05-agentic-workflows/
│   ├── awspec.md                ← M5: Agent Workflow Spec
│   ├── awspec.png               ← M5: AWSpec diagram (earlier draft)
│   ├── agent-control-panel.md   ← M5: Agent Control Panel
│   ├── agent-control-panel.png  ← M5: Control Panel diagram (earlier draft)
│   ├── Juno Agent.json          ← M5: illustrative agent graph, not import-tested
│   └── dry-run/                 ← M5: AWSpec walked through by hand on one example
├── 06-evals/
│   ├── eval-stack.md            ← M6: layered eval stack
│   ├── eval-stack.png           ← M6: eval stack diagram (earlier draft)
│   ├── human-rubric.md          ← M6: human evaluation rubric
│   └── human-rubric.png         ← M6: rubric builder screenshot (earlier draft)
```

---

_Martin Cook · a standalone project, written up in chapters drawn from Product School's AI Product Management course._
