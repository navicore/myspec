(myspec) Examine the codebase and bootstrap the docs/ structure if it doesn't already exist.

1. Read any existing docs/ files, README.md, and CLAUDE.md to understand what's already documented
2. Explore the codebase — look at project structure, key modules, entry points, dependencies, and test patterns
3. Create or fill in the following, skipping any that already exist and are substantive:
   - `docs/ARCHITECTURE.md` — how the system is structured: key modules, their responsibilities, how they connect, important data flows. Write from what the code actually does, not aspirationally.
   - `docs/ROADMAP.md` — current state and known next steps. If there are TODOs, open issues, or incomplete features, capture them here. It's fine if this is short.
   - `mkdir -p docs/design/` — create the directory even if empty

Keep each doc **short and accurate**. One page max. Describe what exists today, not what could exist. These docs will be used by AI coding tools to stay grounded, so precision matters more than polish.

Do NOT:
- Generate boilerplate or placeholder sections
- Add content you're uncertain about — ask me instead
- Overwrite docs that already have real content
- Create docs beyond ARCHITECTURE.md and ROADMAP.md unless I ask

If $ARGUMENTS is provided, also pay attention to: $ARGUMENTS
