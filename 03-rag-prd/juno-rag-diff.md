# RAG before-and-after test · Juno

_Working notes from Module 3: the same escalation prioritised without and then with the strategy one-pager._

**Lovable prototype:** https://lovable.dev/projects/a7059b7a-c63b-49a5-92ea-a899d5941eb0 (private Lovable project, not publicly viewable)

### Before: Quality Mode (no strategy)

- Optimise backend query and CSV serialization for large date ranges (90+ days).

- Implement asynchronous export processing or background job queuing with download notification/link if generation exceeds 15 seconds.

- Implement proper error handling, retry mechanisms, and informative UI error banners instead of blank screen crashes.

### After: Strategy Mode (with RocketShip Strategy One-Pager)

- Investigate and resolve timeout and memory crash issues on the 'Export to CSV' pipeline for date ranges up to 90+ days.

- Implement resilient background processing/streaming for CSV exports to prevent browser hanging.

- Add explicit error logging and user-facing error states in place of silent blank crashes.

### Takeaway

> RAG reworded the priorities but did not change the ranking: same three issues, same order. "Optimise the query" became "find the timeout and memory crash", and "background job queuing" became "prevent browser hanging". This run shows sharper problem statements, not better prioritisation.

