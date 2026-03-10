(myspec) Examine the codebase and bootstrap the docs/ structure if it doesn't already exist.

1. Read any existing docs/ files, README.md, and CLAUDE.md to understand what's already documented
2. Explore the codebase — look at project structure, key modules, entry points, dependencies, and test patterns
3. Create or fill in the following, skipping any that already exist and are substantive:
   - `docs/ARCHITECTURE.md` — structure it around these concepts (inspired by arc42), keeping each section to a few sentences or a short list:
     - **Context & Scope**: what is this system, what are its boundaries, what external systems/APIs/services does it interact with. Draw the line between inside and outside. If the system has multiple bounded contexts, briefly note how they relate — who's upstream, who's downstream, where translation or anticorruption layers exist.
     - **Solution Strategy**: key technology choices and why — language, frameworks, storage, protocols. The decisions that shape everything else.
     - **Building Blocks**: the main modules/crates/packages, their responsibilities, and how they connect. Identify the core domain entities and their invariants — what owns what, what must stay consistent. Where aggregates exist, name the roots and their consistency boundaries — what can only be changed through what. Use the project's domain language — name things the way the code names them.
     - **Crosscutting Concepts**: patterns that span modules — error handling, concurrency model, logging/observability, serialization conventions, anything a contributor needs to follow consistently.
     Write from what the code actually does, not aspirationally.
   - `docs/ROADMAP.md` — current state and known next steps. If there are TODOs, open issues, or incomplete features, capture them here. It's fine if this is short.
   - `mkdir -p docs/design/` — create the directory even if empty

Keep each doc **short and accurate**. One page max. Describe what exists today, not what could exist. These docs will be used by AI coding tools to stay grounded, so precision matters more than polish.

Do NOT:
- Generate boilerplate or placeholder sections
- Add content you're uncertain about — ask me instead
- Overwrite docs that already have real content
- Create docs beyond ARCHITECTURE.md and ROADMAP.md unless I ask

If $ARGUMENTS is provided, also pay attention to: $ARGUMENTS
