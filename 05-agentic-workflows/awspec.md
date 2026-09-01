
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
| 1 | Sequential steps: 1. Read the transcript and identify the pain point. 2. RAG retrieval over the RocketShip Strategy One-Pager, top-K = 6. 3. Compare the pain point against the four strategic pillars. 4. Score risk and alignment, emit P0 to P3 with a confidence score. 5. Draft Insight Cards and PRD section, route to PM based on the confidence threshold. | Tool inventory: | Read/write boundaries: Agent can READ Slack #escalations, the Strategy One-Pager, and Jira ROCKET tickets. Agent can WRITE only draft Insight Cards and PRD sections inside the Juno app, always in draft state. Agent CANNOT publish, export, edit Jira tickets, or post anywhere externally without PM approval, per PRD requirement 6. |
| 2 | _ | slack.read_thread(id), read-only |  |
| 3 | _ | notion.retrieve(strategy_doc_id), read-only |  |
| 4 | _ | jira.read_ticket(key), read-only |  |
| 5 | _ | corpus.retrieve(query, k=6), read-only |  |
| 6 | _ | juno.draft_card(payload), write, always saved as draft, requires PM approval |  |
| 7 | _ | juno.draft_prd_section(payload), write, always saved as draft, requires PM approval |  |

**Schemas**

Schemas:
corpus.retrieve → {chunks: [{text, source, pillar, score}]}
jira.read_ticket → {key, priority, status, summary}
slack.read_thread → {messages: [{text, permalink, timestamp}]}
juno.draft_card → {priority, confidence, citations: [...], status: draft|approved}

**Memory (in or out of scope)**

- **Episodic:** Episodic: In-scope, this run's retrieved chunks, transcript text, intermediate risk and confidence scores. Lifetime: end of run.
- **Semantic:** Semantic: In-scope, RocketShip's four strategic pillars, the grounding and evidence-balance rules, prior PM override patterns per pillar. Lifetime: indefinite, refreshed whenever the Strategy One-Pager changes. Out of scope: do not persist customer PII or contract terms.
- **Working:** Working / contextual: In-scope, current transcript, customer ID if mentioned, retrieved KB chunks, running confidence score. Held in working context only, discarded at end of run.
- **External:** External tools: Slack #escalations API (read), Strategy One-Pager in Notion (read), Jira ROCKET project (read only), Juno app's own draft store (write, always PM-gated before publish).

## Human-in-the-loop

Humans in the loop: PM reviews any priority with confidence under 70% before it's shown as final. PM must click Approve before anything publishes or exports, no exceptions.

## Success & failure

- **Done when:** Stop conditions: Success: ranked cards and PRD draft posted for PM review. Failure: fewer than 3 retrieved segments, or no strategy doc loaded, hands back with an insufficient-evidence banner. Escalation: confidence 30 to 69 tags medium confidence and forces PM review; confidence under 30 returns notRecommended with no ranking produced. Timeout: evidence-balance gate fails twice in a row, falls back to the insufficient-evidence banner rather than retrying indefinitely or shipping a skewed list.
- **Fails safe when:** Read/write boundaries: Agent can READ Slack #escalations, the Strategy One-Pager, and Jira ROCKET tickets. Agent can WRITE only draft Insight Cards and PRD sections inside the Juno app, always in draft state. Agent CANNOT publish, export, edit Jira tickets, or post anywhere externally without PM approval, per PRD requirement 6.

## Self-review

- [ ] Goal is one sentence and names the value frame.
- [ ] Trigger is a precise, testable condition.
- [ ] Pattern is chosen with a defensible reason.
- [ ] At least 3 stop conditions, including escalation.
- [ ] Each memory type named (in or out).
- [ ] Every tool lists scope (read-only vs write) and a schema.
- [ ] Read/write boundaries match the AI PRD (M3).
