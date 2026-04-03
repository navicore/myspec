(myspec) I want to capture a reusable skill from this repo. The skill: $ARGUMENTS

This repo is the **reference implementation** — study how it does the thing described above so the skill captures real detail, not generic advice. The skill file will be copied to other repos.

1. Read $ARGUMENTS to understand the intent of the skill
2. Examine the repo — CI workflows, config files, scripts, Makefiles, Cargo.toml, package.json, docs/, whatever is relevant to the described skill. Understand how this repo actually does it today.
3. Write a `.claude/commands/<skill-name>.md` file that captures the **pattern**, not this repo's specifics:
   - First line: a short description of what the skill does, using `$ARGUMENTS` for user input
   - **Inputs**: what the skill should read in whatever repo it runs in (e.g., "Read Cargo.toml", not "Read myspec's Cargo.toml")
   - **Steps**: what to do, in order — clear, concrete, adapted to the target repo's actual state
   - **Constraints**: what not to do — scope boundaries, things to leave alone
   - **Final step**: always end with "Update relevant docs (ARCHITECTURE.md, ROADMAP.md, or design docs) to reflect what you changed"
   - Include a brief comment at the bottom: `<!-- Reference: pattern extracted from [repo-name] -->`

Keep it under one page. The skill should be specific enough to be useful but general enough to work in a different repo that does something similar.

Do NOT:
- Hardcode repo-specific names, versions, paths, or URLs — use "read the config to find X" instead
- Add steps the user didn't ask for — capture the workflow, don't improve it
- Create docs/ entries — that happens when the skill runs, not when it's made
- Write a skill so generic it could be an LLM prompt — ground it in what you actually see in this repo
