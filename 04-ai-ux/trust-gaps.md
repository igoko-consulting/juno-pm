# Trust-Gap Mitigations · Juno

> Module 4 · AI-Native UX. Trust gaps surfaced with the **M4 · AI-UX Trust Gap Checker**, and how each is mitigated. Paste the tool's markdown over this file.

## Trust gaps

| Gap | Where it shows up | User cost | Mitigation |
|---|---|---|---|
| _Hallucination_ | Juno invents a citation, or stretches a weak match into a P0 call it can't really support. | PM defends a made-up source to leadership. Credibility gone in one meeting, and it's the exact failure the old process already had. | Hard grounding rule, minimum two citations per priority. Fail-safe banner instead of a guess when evidence is thin. Evidence-balance gate stops one loud source drowning out the rest. |
| _Opacity (no "why")_ | A ranked card shows a P0 to P3 badge with no visible reasoning behind it. | PM has to re-derive the logic by hand anyway, which quietly erases the 75% time saving Juno is meant to deliver. | Inline footnotes on every card, clickable back to source. Strategic Traceability footer names the pillar. Breadcrumb messages show what Juno is doing while it works, not just that it's working. |
| _No user control_ | PM disagrees with a ranking and has no way to correct it in place. | PM stops trusting the tool and quietly goes back to manual prioritisation. That's not a bug fix moment, that's adoption dying. | Manual override on every card. PM approval gate blocks anything from publishing without a click. Overrides get logged and feed back into tightening retrieval. |

## Highest-priority fix

Hallucination, close it first.

Opacity and control only matter if what's underneath them is actually true. A well-labelled, easily overridden fake citation is still a fake citation, and it's the one failure mode we already know kills trust fastest, since it's what broke the old process in the first place. Fix the ground truth before polishing how it's shown or how easily it's corrected.
