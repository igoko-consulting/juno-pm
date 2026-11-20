# AI Solution Decision Matrix · Juno

## The decision

Whether RocketShip builds Automated Prioritisation in Juno as a Hybrid (RAG + Agentic) Copilot, vs. buying a generic LLM API or fine-tuning a model on our corpus.

Why now: roadmap discussions are driven by the loudest voice in Slack rather than customer evidence. Priorities reverse weekly, and the PM cannot defend the call to leadership.

## Options scored

| Option | Cost | Speed | Control | Moat | Risk | Score |
|---|---|---|---|---|---|---|
| Build | 2 | 2 | 5 | 5 | 4 | 3.6 |
| Buy / API | 5 | 5 | 2 | 1 | 2 | 3.0 |
| Fine-tune | 3 | 2 | 4 | 4 | 3 | 3.2 |

## Recommendation

Build. Control and Moat carry 2x weight because they decide whether a priority call is defensible to leadership. That widens Build's lead from a thin 0.4-point unweighted margin to 0.57 to 1.43 points over the alternatives.

Buy/API wins on Cost and Speed but can't cite RocketShip sources, reproducing the same loudest-voice dynamic the PM is trying to escape, just with an LLM doing the talking.
Fine-tune is directionally right on Control and Moat but goes stale as the corpus moves and still can't cite a live source the way RAG retrieval can.
Autonomy stays Copilot. RAG grounds every priority in a source ID, bounded agentic orchestration surfaces Jira-vs-Slack conflicts without resolving them, and the PM approves before anything publishes. The architecture could move towards Agent autonomy once Control is validated in production. Moving too early is not reversible in trust terms: one unsupervised priority change would be enough to lose it.

Success is measured directly against the "why now": prioritisation time down 75%, reversal rate under 10%, and 90% evidence coverage. Recommend a checkpoint after the first full prioritisation cycle to confirm these hold before considering any autonomy increase.
