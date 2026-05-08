---
name: pickup
description: Resume work from a /handoff document. Uses the most recent handoff by default, or an explicit path if provided.
argument-hint: "[optional path to a specific handoff file]"
---
Resume work from a previous session's handoff document.

## Locating the handoff

If the user passed an argument, treat it as the explicit path to a handoff file and read that file directly.

If no argument was passed:
1. Read the pointer file at `~/.claude/handoffs/latest` to get the path of the most recent handoff.
2. If the pointer file doesn't exist, tell the user no recent handoff was found and ask whether they want to pass an explicit path.
3. If the pointer file exists but the path it points to no longer exists (e.g. `/tmp` was cleared on reboot), tell the user the handoff has expired and ask how they want to proceed.

## Using the handoff

Read the handoff document and treat its contents as the starting context for this session. Then, before acting on any of it:

- Verify referenced paths, files, URLs, and issue numbers still resolve.
- Check whether work described as "pending" or "next" has already been completed since the handoff was written (e.g. commits landed, issues closed, branches merged).
- Note any drift between the handoff snapshot and current reality, and surface it to the user.

The handoff is a *starting hypothesis*, not gospel. The underlying repo, issues, commits, and docs it references are the source of truth — let reality override the snapshot where they disagree.

## Confirm before continuing

Once you've read the handoff and verified its references, give the user a short summary of:
- What the previous session was working on
- What state things were left in
- What the suggested next steps are (per the handoff)
- Any drift you noticed between the handoff and current reality

Then ask whether to proceed with the suggested next steps or take a different direction.

