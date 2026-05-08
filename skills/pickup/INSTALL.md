# Installing the `handoff` and `pickup` skills (personal scope)

This repo ships two paired skills for Claude Code:

- **`/handoff`** — compacts the current conversation into a handoff document at a tempfile path, and updates a pointer at `~/.claude/handoffs/latest` so the next session can find it.
- **`/pickup`** — resumes work from a handoff document. Uses the pointer file by default, or accepts an explicit path.

Symlinking them into your personal Claude config makes both commands available in **every** project on your machine, and any updates pulled into this repo flow through automatically.

## Prerequisites

- Claude Code installed
- This repo cloned somewhere stable (it shouldn't move after you symlink)

## Install

From the root of this repo:

```bash
mkdir -p ~/.claude/skills
mkdir -p ~/.claude/handoffs

REPO_ROOT="$(pwd)"
ln -s "$REPO_ROOT/skills/handoff" ~/.claude/skills/handoff
ln -s "$REPO_ROOT/skills/pickup"  ~/.claude/skills/pickup
```

Two notes on this:

- `REPO_ROOT="$(pwd)"` captures an absolute path *once*, so both `ln` calls resolve correctly. Symlinks need absolute targets — relative paths break the moment you read the link from a different working directory.
- `~/.claude/handoffs/` is where `/handoff` writes its pointer file. Creating it up front means the first run of `/handoff` won't fail on a missing directory.

## Verify

```bash
ls -la ~/.claude/skills/
```

You should see both symlinks pointing at your repo:

```
handoff -> /Users/you/code/this-repo/skills/handoff
pickup  -> /Users/you/code/this-repo/skills/pickup
```

Then in any directory, start Claude Code and type `/`. Both `handoff` and `pickup` should appear in autocomplete with their descriptions.

## Use

End of a session, before switching context:

```
/handoff
/handoff "what the next session should focus on"
```

Start of the next session:

```
/pickup
/pickup /tmp/handoff-aB3xK9.md
```

`/pickup` with no argument reads `~/.claude/handoffs/latest` to find the most recent handoff. Pass an explicit path when you want a specific older one.

## Update

```bash
cd /path/to/this-repo
git pull
```

Changes take effect in the next Claude Code session. No restart, no re-link.

## Uninstall

```bash
rm ~/.claude/skills/handoff
rm ~/.claude/skills/pickup
```

This removes the symlinks only — the source files in this repo are untouched. Optionally, `rm -rf ~/.claude/handoffs` clears the pointer file and any handoffs written there directly.

## Troubleshooting

**`/handoff` or `/pickup` doesn't appear in autocomplete**

- Check the symlinks resolve: `ls -la ~/.claude/skills/handoff` and `ls -la ~/.claude/skills/pickup` should each show a target path, not "No such file or directory".
- Confirm both files are named `SKILL.md` exactly (case-sensitive).
- Check the YAML frontmatter at the top of each `SKILL.md` — stray indentation or smart quotes will silently break parsing.
- Restart your Claude Code session.

**Symlink points to the wrong place (doubled path, etc.)**

If you ran the install command from inside `skills/handoff/` instead of the repo root, the symlink target will be wrong (you'll see something like `.../skills/handoff/skills/handoff`). Fix:

```bash
rm ~/.claude/skills/handoff ~/.claude/skills/pickup
cd /path/to/this-repo
REPO_ROOT="$(pwd)"
ln -s "$REPO_ROOT/skills/handoff" ~/.claude/skills/handoff
ln -s "$REPO_ROOT/skills/pickup"  ~/.claude/skills/pickup
```

**`/pickup` says no recent handoff was found**

Either no `/handoff` has run yet on this machine, or the pointer file at `~/.claude/handoffs/latest` was cleared. Run `/handoff` from any session to seed it, or pass an explicit path to `/pickup`.

**`/pickup` says the handoff has expired**

The pointer was valid but the tempfile it pointed at is gone — usually because `/tmp` was cleared on reboot. This is expected and intentional (handoffs are designed to evaporate). Either pass an explicit path to a handoff you've stashed elsewhere, or start a fresh planning session.

**Symlink target is broken after moving the repo**

Remove the old symlinks and re-run the install command from the new location:

```bash
rm ~/.claude/skills/handoff ~/.claude/skills/pickup
cd /new/path/to/this-repo
REPO_ROOT="$(pwd)"
ln -s "$REPO_ROOT/skills/handoff" ~/.claude/skills/handoff
ln -s "$REPO_ROOT/skills/pickup"  ~/.claude/skills/pickup
```

**Windows users**

`ln -s` requires either Developer Mode enabled or an elevated shell. Alternatively, use `mklink /D` from `cmd.exe` (run as Administrator):

```cmd
mklink /D "%USERPROFILE%\.claude\skills\handoff" "C:\path\to\this-repo\skills\handoff"
mklink /D "%USERPROFILE%\.claude\skills\pickup"  "C:\path\to\this-repo\skills\pickup"
```

