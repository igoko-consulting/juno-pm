# AI Solution Decision Matrix · Juno

## The decision

Whether RocketShip builds Automated Prioritization in Juno as a Hybrid (RAG + Agentic) Copilot, vs. buying a generic LLM API or fine-tuning a model on our corpus.

Why now: roadmap discussions are driven by the loudest voice in Slack rather than customer evidence. Priorities reverse weekly, and the PM cannot defend the call to leadership.

## Options scored

| Option | Cost | Speed | Control | Moat | Risk | Score |
|---|---|---|---|---|---|---|
| Build | 2 | 2 | 5 | 5 | 4 | 3.6 |
| Buy / API | 5 | 5 | 2 | 1 | 2 | 3.0 |
| Fine-tune | 3 | 2 | 4 | 4 | 3 | 3.2 |

## Recommendation

Build. Weighting Control and Moat at 2x — the axes that determine whether a ranking is defensible to leadership — widens Build's lead from a thin 0.4-point unweighted margin to a clear 0.57–1.43-point margin over the alternatives.

Buy/API wins on Cost and Speed but can't cite RocketShip sources, reproducing the same loudest-voice dynamic the PM is trying to escape, just with an LLM doing the talking.
Fine-tune is directionally right on Control and Moat but goes stale as the corpus moves and still can't cite a live source the way RAG retrieval can.
Autonomy stays Copilot. RAG grounds every ranking in a source ID; bounded agentic orchestration surfaces Jira-vs-Slack conflicts without resolving them; the PM approves before anything publishes. This is a reversible decision — the architecture can absorb a future move toward Agent-level autonomy once Control is validated in production — but crossing into Agent autonomy prematurely is not reversible in the trust sense: one unsupervised priority change is enough to undo it.

Success is measured directly against the "why now": prioritization time down 75%, reversal rate under 10%, and 90% evidence coverage. Recommend a checkpoint after the first full prioritization cycle to confirm these hold before considering any autonomy increase.
