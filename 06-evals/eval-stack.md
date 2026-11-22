# Eval Stack · Juno

_Module 6._

## What "good" means

Juno turns a noisy P0/P1 escalation into a ranked, cited priority a PM can trust and act on without re-deriving the reasoning by hand. The metrics that prove it: citation coverage ≥90%, decision-reversal rate under 10% within a week, and cycle time down from 2 hours to 30 minutes (Strategy One-Pager targets).

**Online signal (Layer 1, User Feedback):**

- **Signals:** Active, PM clicks Approve, Manual Override (re-tag P0 to P3, edit rationale, mark notRecommended), or sends a card back for clarification. Passive, time-to-approve per card, and cards left un-actioned past end of day.
- **Cadence:** Per request (real-time capture) plus weekly aggregate review.
- **Pass bar:** Decision-reversal rate under 10% within 1 week. Manual Override rate under 20% of cards, above that the pillar-matching logic needs review, not just the card.
- **Owner:** PM reviews the weekly aggregate; roadmap-owner PM triages same-day if override rate spikes on one pillar.

## The stack

| Layer | Evaluator | What it catches | Threshold / gate |
|---|---|---|---|
| Code-based | Automated checks, CI gate on every PR plus nightly cron | Format check (Insight Card has priority, confidence, citations, status). Citation check (every citation resolves to a real, current source, PRD requirement 3). Evidence-balance check (no source type over 80% of a batch's citations, PRD requirement 4). | 100% pass on format, citation, and evidence-balance checks. Any single failure blocks the PR. |
| LLM-as-judge | LLM judge scored against the golden set | Pillar-alignment fidelity, rubric-aligned to the human rubric's dimension 3, catches a plausible-but-wrong pillar citation. Fabricated PII, ARR, or customer names, must trigger a refusal, not an answer. Conflict-detection, does Juno flag a golden-set item with a known Jira/Slack mismatch (PRD requirement 5). | ≥90% golden-set accuracy on priority and pillar match. |
| Human | 2 graders (1 PM, 1 engineer) plus a third PM as tiebreak, scored against `06-evals/human-rubric.md` | All P0/P1 runs reaching the PM review queue each week, stratified by confidence band, plus a random 10% of notRecommended outputs, plus 100% of conflicting-signal cases. Graders differing by 2+ points on any dimension escalate to the tiebreak PM with a logged rationale, 15%+ disagreement rate on any dimension triggers re-calibration. | Mean ≥4.0/5 across Accuracy of top-3 risks and Citation grounding, zero scores of 1 on Safety/refusal, Citation grounding ≥4.5/5. |

## Golden set

100 anonymised P0/P1 escalations with PM-curated expected priority, pillar, and confidence band. Versioned in `06-evals/golden-set/`. Refresh quarterly, and immediately after any incident where Juno missed a real Jira/Slack conflict.

## Release gate

**Hard gates (auto-block):**

0% fabricated PII, ARR, or customer names. 0 critical safety fails (any score of 1) on the human-eval Safety/refusal dimension. Any citation-check failure. Anything published without a PM approval click, the Copilot boundary itself, non-negotiable.

**Soft gates (PM sign-off):**

p95 latency over 3 seconds requires PM justification before shipping. Manual Override rate over 20% on a single pillar in a week flags that pillar for review. A rising notRecommended rate week over week requires PM sign-off before continuing.
