# Viewyonder Claude Skills

A Claude Code plugin marketplace by [Viewyonder](https://viewyonder.com).

## Plugins

| Plugin | Description |
|--------|-------------|
| **coherence** | Architectural guardrails — init wizard, drift detection, architecture review, test runner |
| **squirrel** | Token optimization audit — score 10 behaviours, labelled snapshots, trend tracking |

## Installation

```bash
# Add the marketplace
/plugin marketplace add viewyonder/claude-skills

# Install plugins
/plugin install coherence@viewyonder-claude-skills
/plugin install squirrel@viewyonder-claude-skills
```

## Usage

Once installed, plugins are available as slash commands:

- `/coherence:init` — set up architectural guardrails for your project
- `/coherence:status` — check guardrail status
- `/squirrel` — run a token optimization audit
- `/squirrel snapshot "end of sprint"` — save a labelled snapshot
- `/squirrel trend` — view score history

## Links

- [Coherence](https://coherence.viewyonder.com) — full documentation
- [TokenSquirrel](https://github.com/viewyonder/tokensquirrel) — source code and CLI

## License

MIT
