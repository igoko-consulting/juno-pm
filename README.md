# Juno PM

> Juno turns noisy Slack, Jira, and Notion signals into a ranked, cited priority a RocketShip PM can defend, without re-deriving the reasoning by hand.
>
> RocketShip is the fictional company from the course case study, not a real client.

_Martin Cook · AI Product Management Certification · #ai-product-management-aug17-26-weeknights_

This repo is my final project for the **AI Product Management Certification**. Each module's artefact lives in its own folder; this README is the dashboard and the pitch.

**[View the visual pitch →](pitch.html)**

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
- **Agent graph** (illustrative, not import-tested) — [`05-agentic-workflows/Juno Agent.json`](<05-agentic-workflows/Juno Agent.json>)
- **Dry run** (the AWSpec applied by hand to one reconstructed escalation, not run on a live model) — [`05-agentic-workflows/dry-run/`](05-agentic-workflows/dry-run/)

### M6 · Evals & Guardrails
- **Eval stack** — [`06-evals/eval-stack.md`](06-evals/eval-stack.md) ([diagram](06-evals/eval-stack.png))
- **Human evaluation rubric** — [`06-evals/human-rubric.md`](06-evals/human-rubric.md) ([diagram](06-evals/human-rubric.png))

> Naming note: two folders use different filenames than the certification template suggests, content and module mapping are correct, only the names differ.
>
> | Folder | Template suggests | This repo uses |
> |---|---|---|
> | `01-prompting/` | `prototype.md` | `lovable-prototype.md` |
> | `03-rag-prd/` | `ai-prd.md`, `before-after-rag.md` | `prd.md`, `juno-rag-diff.md` |

---

## PM Execution Plan

### Where Juno is today
- All six modules drafted (M1 to M6) and cross-referenced.
- A walkthrough by hand of one reconstructed escalation shows the spec routes a Jira/Slack conflict to PM review even at high confidence.
- Not yet running against a live LLM or in production, and the golden set is not yet built.
- Modelled payback of about four quarters on PM time saved (Strategy One-Pager §2). It excludes running costs, PM review and grading time, and the upside of fewer wrong bets.

### What ships next (next 2 sprints)
- **Sprint 1:** build the 100-item golden set and wire the code-based CI checks (format, citation, evidence-balance). This closes the biggest open gap: accuracy has been shown on one worked case, not at scale.
- **Sprint 2:** stand up a real Langflow instance and validate `Juno Agent.json` against a live model, actual model variance, latency, and tool-call failure modes, the one thing the golden set can't test.

### What I watch (dashboards)
- **Daily:** Manual Override rate, time-to-approve / un-actioned rate, cost per run against the 50-run daily cap (modelled at $6 or more a day).
- **Weekly:** decision-reversal rate, human-rubric mean per dimension, evidence-balance gate trigger rate.
- **Per release** (CI gate + nightly cron): golden-set accuracy, format/citation/evidence-balance pass rate.

### Red lines (what blocks shipping — numbers, not feelings)
- <90% golden-set accuracy on the automated layer.
- 0% fabricated PII, ARR, or customer names.
- 0 critical safety fails (any score of 1) on the human-eval Safety/refusal dimension.
- Any citation-check failure.
- Anything published without a PM approval click.

All five are hard, auto-blocking gates in `06-evals/eval-stack.md`, none are a "PM discretion" call.

### Governance
_Compliance · Safety · Reliability · Reputation._

- **Compliance:** no external comms and no publishing without human review (system prompt Refusal Conditions, PRD requirement 6).
- **Safety:** hard refusal on PII, ARR, contracts, and legal/regulator requests, routed to the human PM.
- **Reliability:** evidence-balance gate blocks any card where one source type supplies more than 80% of citations; p95 latency target under 3s (PRD requirement 1).
- **Reputation:** `04-ai-ux/trust-gaps.md` addresses the hallucination, opacity, and control gaps directly, rather than assuming citations and an approval button are enough on their own.

---

## Build Insights

- **Friction point.** The M5 and M6 tool exports repeatedly scrambled field content into the wrong table rows or cells (the AWSpec steps table, memory labels, rate/cost caps, the human rubric's disagreement protocol, the eval stack's three layers), not a one-off, caught and fixed across several separate passes.
- **Key learning.** A repo-wide consistency audit caught something no single-module review would have: Module 1's system prompt was still describing a different output shape, a ranked top-5 table, than every module built after it, a single scored priority card. Each module was internally fine; the set wasn't consistent with itself.
- **Aha moment.** Reading the AWSpec is not the same as running it. The Jira/Slack conflict-check branch had been described in three separate documents but never actually exercised until the dry run produced a case where high confidence and a live conflict happened on the same card, and confirmed the conflict flag correctly overrode the confidence score rather than the other way round.

---

## Repo structure

```
juno-pm/
├── README.md                    ← this dashboard + pitch
├── 01-prompting/
│   ├── system-prompt.md         ← M1: Juno's system prompt
│   └── lovable-prototype.md     ← M1: prototype link + debrief
├── 02-strategy/
│   ├── decision-matrix.md       ← M2: build / buy / fine-tune call
│   └── strategy-one-pager.md    ← M2: AI strategy one-pager
├── 03-rag-prd/
│   ├── prd.md                   ← M3: AI PRD with retrieval requirements
│   └── juno-rag-diff.md         ← M3: RAG diagnostic working notes
├── 04-ai-ux/
│   ├── user-flow.md             ← M4: AI-native user flow
│   ├── user-flow.png            ← M4: flow diagram
│   └── trust-gaps.md            ← M4: trust-gap mitigations
├── 05-agentic-workflows/
│   ├── awspec.md                ← M5: Agent Workflow Spec
│   ├── awspec.png               ← M5: AWSpec diagram
│   ├── agent-control-panel.md   ← M5: Agent Control Panel
│   ├── agent-control-panel.png  ← M5: Control Panel diagram
│   ├── Juno Agent.json          ← M5: illustrative agent graph, not import-tested
│   └── dry-run/                 ← M5: AWSpec walked through by hand on one example
├── 06-evals/
│   ├── eval-stack.md            ← M6: layered eval stack
│   ├── eval-stack.png           ← M6: eval stack diagram
│   ├── human-rubric.md          ← M6: human evaluation rubric
│   └── human-rubric.png         ← M6: rubric diagram
```

---

_Certification submission — AI Product Management Certification._
