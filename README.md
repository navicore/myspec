# myspec

Lightweight project spec conventions for working with AI coding tools. No framework, no CLI, no npm install — just docs and slash commands.

For Claude Code, install is something like:

```
mkdir -p .claude/commands && cp -R ~/myspec/.claude/commands/* .claude/commands/
```

## The idea

The problem with AI coding sessions isn't generating specs — it's **drift**. The model loses context, docs go stale, code diverges from intent. The fix is making it easy to pull your docs back into context and compare against reality.

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

Copy `.claude/commands/` into any project. These are re-grounding commands — they pull docs back into context and compare against the codebase.

| Command | What it does |
|---------|-------------|
| `/check` | Compares code against docs. Reports drift, gaps, and staleness. |
| `/design <change>` | Writes a short design doc before coding starts. |
| `/review` | Reviews uncommitted changes against project docs. |
| `/sync-docs` | Updates docs to match current code reality. |
| `/scope <work>` | Establishes what's in and out of scope for a task. |

## Workflow

There is no rigid workflow. Use what helps:

- Starting a feature? `/design add widget support` then `/scope add widget support`
- Mid-session context feeling stale? `/check`
- About to wrap up? `/review`
- Docs fallen behind? `/sync-docs`

## Why not speckit/openspec?

Those tools generate directory structures and enforce phase gates. Under the hood they're just prompt templates in `.claude/commands/` — the same mechanism used here, wrapped in a CLI and an npm package. They don't solve the drift problem because they focus on generating specs, not on continuously re-checking code against specs.
