# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A Claude Code plugin marketplace by Viewyonder. It hosts plugins that users install via `/plugin marketplace add viewyonder/claude-skills` and then `/plugin install <name>@viewyonder-claude-skills`.

There is no build step, no test runner, and no dependencies to install. The repo is pure configuration: JSON manifests and Markdown skill definitions.

## Structure

```
marketplace.json                         # Top-level plugin registry (list of plugins + versions)
.claude-plugin/marketplace.json          # Duplicate of above (required by plugin system)
plugins/
  <name>-plugin/
    .claude-plugin/plugin.json           # Plugin metadata (name, version, author, keywords)
    skills/
      <skill-name>/SKILL.md             # Skill definition (frontmatter + instructions)
```

**Two manifests must stay in sync**: `marketplace.json` and `.claude-plugin/marketplace.json` are identical files. When updating one, update both.

## Plugins

- **coherence** (v1.12.1) — Architectural guardrails: init wizard, drift detection, principle checking, test runner, hooks, agents, and skills generation. Multi-skill plugin with 10 skills.
- **squirrel** (v0.1.0) — Token optimization audit: scores 10 behaviours, labelled snapshots, trend tracking. Single-skill plugin.

## Adding a New Plugin

1. Create `plugins/<name>-plugin/.claude-plugin/plugin.json` with name, description, version, author, and `"skills": "./skills/"`
2. Create `plugins/<name>-plugin/skills/<skill-name>/SKILL.md` for each skill
3. Add the plugin entry to both `marketplace.json` and `.claude-plugin/marketplace.json`

## Skill File Format

Skills use YAML frontmatter followed by Markdown instructions:

```markdown
---
name: <skill-name>
description: <trigger description for when this skill should activate>
user_invocable: true
arguments: "[optional args]"
---

# /<plugin>:<skill-name>

Instructions for Claude when this skill is invoked...
```

The `description` field is critical — it determines when Claude Code activates the skill. Write it as a trigger pattern, not a summary.

## Validation

When modifying plugins, verify:
- Both marketplace JSON files are valid JSON with matching content
- Every plugin.json is valid JSON
- Skill SKILL.md files have valid YAML frontmatter
- Hook templates (in coherence skills) pass `node -c` syntax checking
