# Sample transcript · #escalations, THREAD-2201

> Reconstructed, not the verbatim original. The real raw thread that produced this issue lives only in the Lovable session history, not in this repo. This reconstruction is written to be consistent with the known input/output pair already in [`03-rag-prd/juno-rag-diff.md`](../../03-rag-prd/juno-rag-diff.md), same underlying issue, same "Before" priorities, so this run can be checked against that earlier test.

**Tagged:** P0
**Channel:** #escalations
**Time:** 09:14

---

**@sarah.ops:** Board deck export is broken again. Anyone tried pulling a 90-day CSV export this morning?

**@marcus.cs:** Yep, just tried. Screen goes blank after about 20 seconds, no error, nothing. Had to refresh and lost the filter state too.

**@sarah.ops:** Same. Customer (Pearson Co) is asking for a full quarter export ahead of their QBR tomorrow and it's just not coming back. No timeout message, no retry option, just a dead screen.

**@raj.eng:** Looking at it now. Looks like the query itself is timing out server-side on anything past ~60 days, the frontend has no idea and just hangs. We're not catching the failure anywhere in the export path.

**@sarah.ops:** This is the third time this month someone's flagged this for a QBR-adjacent export. Tagging P0, it's blocking a customer-facing deliverable with a hard deadline.
