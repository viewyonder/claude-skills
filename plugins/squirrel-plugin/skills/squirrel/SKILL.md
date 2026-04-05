---
name: squirrel
description: Run a TokenSquirrel audit to score token optimization habits, take a labelled snapshot, or view score trends. Use when the user says "squirrel", "token audit", "score my session", "how am I doing on tokens", "snapshot my scores", or "show my trend".
---

# /squirrel

Audit Claude Code token optimization habits using TokenSquirrel. Score 10 behaviours (0-100, A-F), save labelled snapshots as progress markers, and view trends over time.

## Tool Path

The CLI lives at `~/dev/viewyonder/tokensquirrel/tools/claude-squirrel/`. All commands use:

```
cd ~/dev/viewyonder/tokensquirrel/tools/claude-squirrel && npx tsx src/index.ts [args]
```

## Subcommands

### `/squirrel` — Run scorecard

Run a full audit and present the results.

```bash
cd ~/dev/viewyonder/tokensquirrel/tools/claude-squirrel && npx tsx src/index.ts --format md
```

Present the markdown output directly to the user. The result is saved to history automatically.

### `/squirrel snapshot <label>` — Labelled snapshot

Run the audit and tag it with a label for future reference. The label marks a meaningful moment — end of a day, sprint, feature, pomodoro session, etc.

```bash
cd ~/dev/viewyonder/tokensquirrel/tools/claude-squirrel && npx tsx src/index.ts --format md --label "<label>"
```

If the user writes `/squirrel snapshot` without a label, ask what to call this snapshot before running.

After running, confirm: "Snapshot saved: **<label>** (score: XX, grade: X)"

### `/squirrel trend` — Show score history

Display the historical trend of audit scores. Labelled snapshots appear with their label in the table.

```bash
cd ~/dev/viewyonder/tokensquirrel/tools/claude-squirrel && npx tsx src/index.ts --trend --format md
```

Accept an optional count: `/squirrel trend 5` passes `--trend 5`.

## Options

The user may combine options with any subcommand. Pass these through as CLI args:

- `--weeks N` — only score the last N weeks of data
- `--project X` — filter to sessions matching a project name
- `--no-save` — run without saving to history

Examples:
- `/squirrel --weeks 2` becomes `--format md --weeks 2`
- `/squirrel snapshot "end of day" --weeks 1` becomes `--format md --label "end of day" --weeks 1`

## Output

Always use `--format md` since output renders inside Claude Code. Present the markdown output directly — do not wrap it in a code block.

## Errors

If the command fails:
1. Check dependencies are installed: `cd ~/dev/viewyonder/tokensquirrel/tools/claude-squirrel && npm install`
2. Check that `~/.claude/` exists and contains `history.jsonl` and `stats-cache.json`
3. Report the error to the user with the failing command and output
