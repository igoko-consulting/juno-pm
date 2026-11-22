## Diagnostic Diff · Juno RAG Lab

_Working notes from Module 3 Lab 1. Do not paste over `03-rag-prd/prd.md`. That file comes from the AI PRD Builder._

**Lovable prototype:** https://lovable.dev/projects/a7059b7a-c63b-49a5-92ea-a899d5941eb0

### Before - Quality Mode (no strategy)

- Optimise backend query and CSV serialization for large date ranges (90+ days).

- Implement asynchronous export processing or background job queuing with download notification/link if generation exceeds 15 seconds.

- Implement proper error handling, retry mechanisms, and informative UI error banners instead of blank screen crashes.

### After - Strategy Mode (with RocketShip Strategy One-Pager)

- Investigate and resolve timeout and memory crash issues on the 'Export to CSV' pipeline for date ranges up to 90+ days.

- Implement resilient background processing/streaming for CSV exports to prevent browser hanging.

- Add explicit error logging and user-facing error states in place of silent blank crashes.

### Takeaway

> The RAG update. it reworded priorities: "optimise the query" became "find the timeout and memory crash," "background job queuing" became "prevent browser hanging." i.e. he ranking is identical pre/post (same three issues, same order), so this run doesn't demonstrate RAG changing what gets prioritized — only the diagnostic precision of how it's described

