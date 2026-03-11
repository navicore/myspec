(myspec) Compare code against docs. Report drift, gaps, and staleness.

If $ARGUMENTS is provided, focus on: $ARGUMENTS

1. Re-read all docs in docs/ and examine the current codebase. Skip docs/design/done/ — those are completed decision records, not active intent.
2. Report:
   - **Drift**: code that contradicts or has moved beyond what docs describe
   - **Gaps**: things docs promise that aren't implemented
   - **Staleness**: doc content describing things that no longer exist
3. Be specific — cite file paths and doc sections. Don't suggest rewrites, just report.
