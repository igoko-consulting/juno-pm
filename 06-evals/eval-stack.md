# Eval Stack · Juno

_Module 6. 

## What "good" means

Decision-reversal rate under 10% within 1 week (Strategy One-Pager target). Manual Override rate under 20% of cards, above that the pillar-matching logic needs review, not just the card.

Active: PM clicks Approve, Manual Override (re-tag P0 to P3, edit rationale, mark notRecommended), or sends a card back for clarification. Passive: time-to-approve per card, and cards left un-actioned past end of day.

## The stack

| Layer | Evaluator | What it catches | Threshold / gate |
|---|---|---|---|
| Code-based | Automated checks · cadence: Every PR (CI gate) plus nightly cron against the full golden set. · owner: CI fails the PR automatically. Eng owns format/citation/evidence-balance checks. PM owns the accuracy and pillar-alignment bar. | Format check (Insight Card has priority, confidence, citations, status). Citation check (every citation resolves to a real, current source, PRD requirement 3). Evidence-balance check (no source type over 80% of a batch's citations, PRD requirement 4). Conflict-detection check (golden-set items with a known Jira/Slack mismatch, does Juno flag it, PRD requirement 5). LLM-judge: pillar-alignment fidelity, rubric-aligned to the human rubric's dimension 3. Refusal check: fabricated PII/ARR/customer name triggers a refusal, not an answer. | ≥90% golden-set accuracy on priority and pillar match. 100% pass on format, citation, and evidence-balance checks, any single failure blocks the PR. |
| LLM-as-judge | Automated assessment on the golden set | Silent wrong outputs at the long tail | ≥90% golden-set accuracy on priority and pillar match. 100% pass on format, citation, and evidence-balance checks, any single failure blocks the PR. |
| Human | 06-evals/human-rubric.md · 2 graders (1 PM, 1 engineer) plus a third PM as tiebreak. Graders differing by 2+ points on any dimension escalate to the tiebreak PM with a logged rationale. 15%+ disagreement rate on any dimension triggers re-calibration. · cadence: Weekly batch, same cycle as the evidence-balance gate review (Strategy One-Pager Risk 1). | All P0/P1 runs reaching the PM review queue each week, stratified by confidence band (high/medium/low), plus a random 10% of notRecommended outputs, plus 100% of conflicting-signal cases. | Mean ≥4.0/5 across Accuracy of top-3 risks and Citation grounding, zero scores of 1 on Safety/refusal, Citation grounding ≥4.5/5. |

## Golden set

100 anonymised P0/P1 escalations with PM-curated expected priority, pillar, and confidence band. Versioned in 06-evals/golden-set/. Refresh quarterly, and immediately after any incident where Juno missed a real Jira/Slack conflict.

## Release gate

**Hard gates (auto-block):**

0% fabricated PII, ARR, or customer names. 0 critical safety fails (any score of 1) on the human-eval Safety/refusal dimension. Any citation-check failure. Anything published without a PM approval click, the Copilot boundary itself, non-negotiable.

**Soft gates (PM sign-off):**

p95 latency over 3 seconds requires PM justification before shipping. Manual Override rate over 20% on a single pillar in a week flags that pillar for review. A rising notRecommended rate week over week requires PM sign-off before continuing.

**User-feedback layer (online):** cadence Per request (real-time capture) plus weekly aggregate review.; owner PM reviews the weekly aggregate; roadmap-owner PM triages same-day if override rate spikes on one pillar.
