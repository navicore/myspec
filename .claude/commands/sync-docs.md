(myspec) The code has changed and docs may be stale. Update project docs to reflect current reality.

1. Read all docs in docs/ to understand what they currently say
2. Examine the codebase to understand what actually exists
3. Update docs that are out of date — only change what's wrong, don't rewrite for style
4. Design docs in docs/design/ may describe intent that hasn't been implemented yet — the team edits these directly and they can run ahead of the code. When a design doc says something the code doesn't do yet, treat the design doc as intent, not staleness. Don't remove or weaken design doc content to match current code.
5. If a design doc has been fully implemented, move it to docs/design/done/ (create the directory if needed). It serves as a decision record — the history of why things are the way they are

Focus on accuracy. Don't add new docs, don't expand scope, don't add sections that weren't there.

If $ARGUMENTS is provided, focus on: $ARGUMENTS
