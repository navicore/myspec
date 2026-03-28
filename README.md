# myspec

Lightweight project spec conventions for working with AI coding tools. No
framework, no CLI, no npm install — just docs and slash commands.

The commands are packaged for Claude Code's `.claude/commands/` convention, but
the approach is portable. The commands are just prompt templates — short
markdown files that tell the model what to do. Any agent that supports custom
instructions or slash commands can use the same prompts.

### Claude Code Install

My own zsh alias is:

```zsh
which myspec
myspec: aliased to  mkdir -p .claude/commands && cp -R ~/git/navicore/myspec/.claude/commands/* .claude/commands/
```

### VS Code Copilot Chat Install

Amazingly copilot-cli doesn't support an user slash commands but VS Code sort of
does as of 3/2026.

VS Code Copilot Chat supports reusable prompt files via `.github/prompts/`. Copy
the commands there as `.prompt.md` files and they'll appear in the prompt picker
(or attach them with `#` in chat):

```zsh
mkdir -p .github/prompts
for f in ~/git/navicore/myspec/.claude/commands/*.md; do
  cp "$f" ".github/prompts/$(basename "${f%.md}").prompt.md"
done
```

Invocation isn't a slash command — you select from the picker or type `#check.prompt` — but it's still explicit and on-demand, not ambient. Copilot CLI doesn't yet have an equivalent directory convention.

### GitHub Actions

Example workflows are in `examples/github/workflows/`. These use
[claude-code-action](https://github.com/anthropics/claude-code-action) to run
Claude Code on pull requests and `@claude` mentions.

The PR review workflow includes a lightweight myspec hint: if `docs/ARCHITECTURE.md`
or `docs/design/` files exist, it checks whether the PR is consistent with them.
This is deliberately minimal — Claude Code GitHub Actions break down with
multi-step prompts, so the docs-awareness is two sentences, not an orchestration
plan. The `@claude` mention workflow has no custom prompt at all and relies on
CLAUDE.md for project context.

Copy the examples to `.github/workflows/` and add a `CLAUDE_CODE_OAUTH_TOKEN`
secret to your repo.

## The idea

The problem with AI coding sessions isn't generating specs — it's **drift**. The
model loses context, docs go stale, code diverges from intent. The fix is making
it easy to pull your docs back into context and compare against reality.

## Structure

```
your-project/
├── CLAUDE.md                     # project rules and conventions
├── docs/
│   ├── ARCHITECTURE.md           # how the system is structured
│   ├── ROADMAP.md                # where it's going
│   └── design/
│       └── feature-name.md       # intent + constraints for a specific change
└── .claude/
    └── commands/                 # slash commands (see below)
```

## Commands

Copy `.claude/commands/` into any project. These are re-grounding commands —
they pull docs back into context and compare against the codebase.

| Command | What it does |
|---------|-------------|
| `/check` | Compares code against docs. Reports drift, gaps, and staleness. |
| `/design <change>` | Plans a change — writes a design doc to `docs/design/` before coding starts. |
| `/scope <work>` | Focuses a session — loads relevant docs and draws boundaries for what's in and out of scope. |
| `/review` | Reviews uncommitted changes against project docs. |
| `/sync-docs` | Updates docs to match current code reality. |
| `/bootstrap` | Examines the codebase and creates initial ARCHITECTURE.md and ROADMAP.md. |

Commands are tagged `(myspec)` in their descriptions to avoid confusion with
built-in commands. Pick names that don't collide with built-ins — e.g.,
`bootstrap` instead of `init`.

## Workflow

There is no rigid workflow. Use what helps:

- Need to think through a change? `/design add widget support` — writes a design doc
- Starting a session on existing work? `/scope add widget support` — loads context and draws boundaries
- Mid-session context feeling stale? `/check` — reports what's drifted
- About to wrap up? `/review`
- Docs fallen behind? `/sync-docs` — fixes the docs to match reality

`/design` and `/scope` serve different moments. `/design` is for planning — use
it when you haven't thought a change through yet. `/scope` is for focusing — use
it when you know what you're doing and just need the relevant docs loaded. If
you wrote a design doc last session, `/scope` will pick it up.

Design docs in `docs/design/` are team artifacts, not model output. `/design`
creates a first draft in collaboration with the model, but the team should own
it from there — edit it, argue over it, update it as you learn. The model starts
the conversation; it doesn't get the last word.

`/check` and `/sync-docs` are also a pair. `/check` is read-only — it tells you
what's drifted without changing anything. `/sync-docs` acts — it updates the
docs to match the code. Use `/check` first to see the state, then `/sync-docs`
if the docs are what's stale (not the code).

## Influences

The `/bootstrap` command coaches architecture docs toward a structure drawn from
[arc42](https://arc42.org) and [Domain-Driven
Design](https://www.domainlanguage.com/ddd/):

- **arc42** — Context & Scope, Solution Strategy, Building Blocks, and
  Crosscutting Concepts provide a proven skeleton for architecture docs without
  the ceremony of a full 12-section template.
- **DDD** — use the project's domain language everywhere (ubiquitous language),
  draw clear system boundaries (bounded contexts), and document what owns what
  and what must stay consistent (entities and invariants).

These aren't enforced as methodology — they're just good defaults for the kind
of architecture doc that keeps an AI coding tool grounded.

## Your docs are the spec

You don't need a tool to generate specs. If you already have
`docs/ARCHITECTURE.md`, `docs/ROADMAP.md`, a `docs/TESTING.md`, design docs —
that's the spec. It's project knowledge you've built up from real experience,
not something an AI should reinvent from a template each time.

The commands pick up whatever is in `docs/`. Add a testing guide, an
observability guide, ADRs — whatever you've found works. The commands will
include it when checking, reviewing, and scoping.

The pattern is: **copy docs that worked from successful projects, not specs
generated by tools.**

## What this can't fix

Good software comes from iterative discovery by cross-discipline teams — people
with different knowledge arguing, building, watching things fail, and adjusting.
DDD calls this knowledge crunching. Arc42 assumes it already happened. Neither
is a phase you can skip or automate.

"Spec-driven development" gets this backwards. It treats the spec as the
starting point when it's actually a byproduct — a snapshot of what the team has
learned so far. Front-loading a detailed spec before you've built anything is
waterfall with better formatting. Building the software is what reveals the
risks — the spec just records what you've learned so far.

Agile was supposed to address this, but most shops practice a choreographed
version that misses the point. "Self-organizing" literally means self-organizing
— the team discovers its own structure, process, and priorities through the
work. That's rare. What's common is standups, sprints, and story points bolted
onto a plan that was locked before the first line of code.

myspec can't make your team iterative or self-organizing. What it can do is
close the loop between what the team has learned and what the model acts on.
When docs drift from reality, the model generates code against stale intent —
wrong boundaries, missed constraints, orphaned abstractions. `/check` surfaces
that drift. `/sync-docs` fixes it. `/design` gives you a place to think before
coding, not a contract to code against.

The value isn't correct docs for their own sake — it's that the model makes
better software when it's grounded against what the team actually knows right
now.

## Why not speckit/openspec?

Those tools generate directory structures and enforce phase gates. Under the
hood they're just prompt templates in `.claude/commands/` — the same mechanism
used here, wrapped in a CLI and an npm package. They don't solve the drift
problem because they focus on generating specs, not on continuously re-checking
code against specs.

AI coding tools work by feeding everything — your instructions, docs,
conversation, tool results — into a context window. That context is finite and
compacts over time. Things fall out. The model forgets your rules, your
constraints, your intent. myspec's commands exist to pull that knowledge back in
when it matters.

That said — if model providers start using post-training to make models treat
certain file conventions with higher weight or special semantics, that changes
the calculus. Worth watching. For now, plain text is plain text.

