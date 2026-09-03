
# Agent Workflow Spec (AWSpec) · Juno

_Module 5. 

## Goal

Goal: Score incoming P0/P1 transcripts against RocketShip's strategic pillars and produce a ranked, cited priority (P0 to P3) with a strategic-rationale citation per item (Trust + risk mitigation).

**Primary actor:** Agent + Human-in-the-loop

## Trigger

Trigger: New transcript added to the Raw Input column, tagged P0 or P1.

## Steps & tools

**Pattern:** ReAct (single-agent reason-act-observe loop)

| Step | Action | Tool / model | Guardrail |
|---|---|---|---|
| 1 | Read the transcript and identify the pain point. | slack.read_thread(id), read-only | |
| 2 | RAG retrieval over the RocketShip Strategy One-Pager, top-K = 6. | corpus.retrieve(query, k=6), read-only | |
| 3 | Compare the pain point against the four strategic pillars. | Reasoning step, no tool call | |
| 4 | Cross-check Jira priority against the Slack escalation for conflicts. | jira.read_ticket(key), read-only | Surfaces conflicts, does not resolve them (PRD requirement 5). |
| 5 | Score risk and alignment, emit P0 to P3 with a confidence score. | Reasoning step, no tool call | |
| 6 | Draft the Insight Card and PRD section, route to PM based on the confidence threshold. | juno.draft_card(payload), juno.draft_prd_section(payload), write, always saved as draft | Agent CANNOT publish, export, edit Jira tickets, or post anywhere externally without PM approval (PRD requirement 6). |

**Schemas**

Schemas:
corpus.retrieve → {chunks: [{text, source, pillar, score}]}
jira.read_ticket → {key, priority, status, summary}
slack.read_thread → {messages: [{text, permalink, timestamp}]}
juno.draft_card → {priority, confidence, citations: [...], status: draft|approved}

**Memory (in or out of scope)**

- **Episodic:** in-scope, this run's retrieved chunks, transcript text, intermediate risk and confidence scores. Lifetime: end of run.
- **Semantic:** in-scope, RocketShip's four strategic pillars, the grounding and evidence-balance rules, prior PM override patterns per pillar. Lifetime: indefinite, refreshed whenever the Strategy One-Pager changes. Out of scope: do not persist customer PII or contract terms.
- **Working / contextual:** in-scope, current transcript, customer ID if mentioned, retrieved KB chunks, running confidence score. Held in working context only, discarded at end of run.
- **External tools:** Slack #escalations API (read), Strategy One-Pager in Notion (read), Jira ROCKET project (read only), Juno app's own draft store (write, always PM-gated before publish).

## Human-in-the-loop

Humans in the loop: PM reviews any priority with confidence under 70% before it's shown as final. PM must click Approve before anything publishes or exports, no exceptions.

## Success & failure

- **Done when:**
  - Success: ranked cards and PRD draft posted for PM review.
  - Failure: fewer than 3 retrieved segments, or no strategy doc loaded, hands back with an insufficient-evidence banner.
  - Escalation: confidence 30 to 69 tags medium confidence and forces PM review; confidence under 30 returns notRecommended with no ranking produced.
  - Timeout: evidence-balance gate fails twice in a row, falls back to the insufficient-evidence banner rather than retrying indefinitely or shipping a skewed list.
- **Fails safe when:** RAG finds no match for the transcript's pain point against any of the four strategic pillars, the card is tagged "Outside current strategy" with an amber warning rather than a forced alignment. PM promotes it manually or sends it back for clarification.

## Self-review

- [x] Goal is one sentence and names the value frame.
- [x] Trigger is a precise, testable condition.
- [x] Pattern is chosen with a defensible reason.
- [x] At least 3 stop conditions, including escalation.
- [x] Each memory type named (in or out).
- [x] Every tool lists scope (read-only vs write) and a schema.
- [x] Read/write boundaries match the AI PRD (M3).
