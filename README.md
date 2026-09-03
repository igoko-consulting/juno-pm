# Juno PM

> Juno turns noisy Slack, Jira, and Notion signals into a ranked, cited priority a RocketShip PM can defend, without re-deriving the reasoning by hand.

_Martin Cook · AI Product Management Certification · [cohort / submission date]_

This repo is my final project for the **AI Product Management Certification**. Each module's artefact lives in its own folder; this README is the dashboard and the pitch.

---

## Module artefacts

### M1 · Prompting
- **System prompt** — [`01-prompting/system-prompt.md`](01-prompting/system-prompt.md)
- **Lovable prototype** — [`01-prompting/lovable-prototype.md`](01-prompting/lovable-prototype.md), [live prototype link](https://id-preview--a7059b7a-c63b-49a5-92ea-a899d5941eb0.lovable.app/)

### M2 · Strategy
- **Decision matrix** — [`02-strategy/decision-matrix.md`](02-strategy/decision-matrix.md)
- **AI Strategy one-pager** — [`02-strategy/strategy-one-pager.md`](02-strategy/strategy-one-pager.md)

### M3 · RAG / AI PRD
- **AI PRD** — [`03-rag-prd/prd.md`](03-rag-prd/prd.md)
- **RAG diagnostic diff (working notes)** — [`03-rag-prd/juno-rag-diff.md`](03-rag-prd/juno-rag-diff.md)

### M4 · AI-Native UX
- **AI user flow** — [`04-ai-ux/user-flow.md`](04-ai-ux/user-flow.md) ([diagram](04-ai-ux/user-flow.png))
- **Trust-gap mitigations** — [`04-ai-ux/trust-gaps.md`](04-ai-ux/trust-gaps.md)

### M5 · Agentic Workflows
- **Agent Workflow Spec (AWSpec)** — [`05-agentic-workflows/awspec.md`](05-agentic-workflows/awspec.md) ([diagram](05-agentic-workflows/awspec.png))
- **Agent Control Panel** — [`05-agentic-workflows/agent-control-panel.md`](05-agentic-workflows/agent-control-panel.md) ([diagram](05-agentic-workflows/agent-control-panel.png))
- **Agent graph** — [`05-agentic-workflows/Juno Agent.json`](<05-agentic-workflows/Juno Agent.json>)
- **Dry run** (AWSpec executed end to end, no Langflow, no API key) — [`05-agentic-workflows/dry-run/`](05-agentic-workflows/dry-run/)

### M6 · Evals & Guardrails
- **Eval stack** — [`06-evals/eval-stack.md`](06-evals/eval-stack.md) ([diagram](06-evals/eval-stack.png))
- **Human evaluation rubric** — [`06-evals/human-rubric.md`](06-evals/human-rubric.md) ([diagram](06-evals/human-rubric.png))

---

## PM Execution Plan

### Where Juno is today
All six modules are built and, after a full repo-wide consistency pass, internally consistent with each other, one product description end to end, not six independently plausible ones. The AWSpec's logic has been proven with a real dry run against a live example (`05-agentic-workflows/dry-run/`), including the Jira/Slack conflict-check branch. Not yet running against a live LLM or in production.

### What ships next (next 2 sprints)
- **Sprint 1:** stand up a real Langflow instance and validate `Juno Agent.json` against a live model, the one thing the dry run couldn't test, actual model variance, latency, and tool-call failure modes.
- **Sprint 2:** build the 100-item golden set (`06-evals/golden-set/`) and wire the code-based CI checks (format, citation, evidence-balance) from the eval stack.

### What I watch (dashboards)
Cycle time, decision-reversal rate, citation coverage, confidence-band distribution, evidence-balance gate trigger rate, and Manual Override rate per strategic pillar.

### Red lines (what blocks shipping — numbers, not feelings)
0% fabricated PII, ARR, or customer names. 0 critical safety fails (any score of 1) on the human-eval Safety/refusal dimension. Any citation-check failure. Anything published without a PM approval click. All four are hard, auto-blocking gates in `06-evals/eval-stack.md`, none are a "PM discretion" call.

### Governance
_Compliance · Safety · Reliability · Reputation._

- **Compliance:** no external comms and no publishing without human review (system prompt Refusal Conditions, PRD requirement 6).
- **Safety:** hard refusal on PII, ARR, contracts, and legal/regulator requests, routed to the human PM.
- **Reliability:** fail-safe banners and the evidence-balance gate stop Juno shipping ungrounded or skewed output rather than guessing.
- **Reputation:** `04-ai-ux/trust-gaps.md` addresses the hallucination, opacity, and control gaps directly, rather than assuming citations and an approval button are enough on their own.

---

## Build Insights

- **Friction point.** The M5 and M6 tool exports repeatedly scrambled field content into the wrong table rows or cells (the AWSpec steps table, memory labels, rate/cost caps, the human rubric's disagreement protocol, the eval stack's three layers), not a one-off, caught and fixed across several separate passes.
- **Key learning.** A repo-wide consistency audit caught something no single-module review would have: Module 1's system prompt was still describing a different output shape, a ranked top-5 table, than every module built after it, a single scored priority card. Each module was internally fine; the set wasn't consistent with itself.
- **Aha moment.** Reading the AWSpec is not the same as running it. The Jira/Slack conflict-check branch had been described in three separate documents but never actually exercised until the dry run produced a case where high confidence and a live conflict happened on the same card, and confirmed the conflict flag correctly overrode the confidence score rather than the other way round.

---

## Repo structure


---

## Repo structure

```
juno-pm/
├── README.md ← this dashboard + pitch
├── 01-prompting/
│ ├── system-prompt.md ← M1: Juno's system prompt
│ └── lovable-prototype.md ← M1: prototype link + debrief
├── 02-strategy/
│ ├── decision-matrix.md ← M2: build / buy / fine-tune call
│ └── strategy-one-pager.md ← M2: AI strategy one-pager
├── 03-rag-prd/
│ ├── prd.md ← M3: AI PRD with retrieval requirements
│ └── juno-rag-diff.md ← M3: RAG diagnostic working notes
├── 04-ai-ux/
│ ├── user-flow.md ← M4: AI-native user flow
│ ├── user-flow.png ← M4: flow diagram
│ └── trust-gaps.md ← M4: trust-gap mitigations
├── 05-agentic-workflows/
│ ├── awspec.md ← M5: Agent Workflow Spec
│ ├── awspec.png ← M5: AWSpec diagram
│ ├── agent-control-panel.md ← M5: Agent Control Panel
│ ├── agent-control-panel.png ← M5: Control Panel diagram
│ ├── Juno Agent.json ← M5: agent graph, realigned to the AWSpec
│ └── dry-run/ ← M5: AWSpec executed live, no Langflow needed
├── 06-evals/
│ ├── eval-stack.md ← M6: layered eval stack
│ ├── eval-stack.png ← M6: eval stack diagram
│ ├── human-rubric.md ← M6: human evaluation rubric
│ └── human-rubric.png ← M6: rubric diagram
```

---

_Certification submission — AI Product Management Certification._
