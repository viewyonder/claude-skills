---
name: handoff
description: Compact the current conversation into a handoff document for another agent to pick up.
argument-hint: "What will the next session be used for?"
---
Write a handoff document summarising the current conversation so a fresh agent can continue the work.

1. Generate the handoff path with `mktemp /tmp/handoff-XXXXXX.md`.
2. Read the file before you write to it (it will be empty, but this confirms the path is writable).
3. Write the handoff content to that path.
4. Update the pointer file so `/pickup` can find this handoff: `mkdir -p ~/.claude/handoffs && echo "<path>" > ~/.claude/handoffs/latest` (substituting the actual mktemp path).
5. Print the handoff path to the user so they have it if they want to pass it explicitly.

Suggest the skills to be used, if any, by the next session.

Do not duplicate content already captured in other artifacts (PRDs, plans, ADRs, issues, commits, diffs). Reference them by path or URL instead. The handoff doc is the *delta* — the conversational state and decisions that exist nowhere else.

If the user passed arguments, treat them as a description of what the next session will focus on and tailor the doc accordingly.

