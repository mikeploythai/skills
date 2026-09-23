# Setup Claude

Install, update, preview, or explain mikeploythai's opinionated Claude Code multi-agent setup.

See [SKILL.md](SKILL.md) for the installation workflow and [configuration](references/configuration.md) for the settings and subagent definitions.

The setup adds Mike's way and CodeGraph guidance to the global `CLAUDE.md`. Install [Mike's way](../mikes-way/README.md) for its default instruction to work. CodeGraph is optional. This is the Claude Code counterpart to [Setup Codex](../setup-codex/README.md).

## Installation

With Node.js and npm available, run:

```sh
npx skills add mikeploythai/skills --skill setup-claude
```

Follow the prompts to choose the installation scope. See the [Skills CLI docs](https://skills.sh/docs/cli) for other options.

## Usage

Explicitly ask your agent to preview, install, or update the setup:

```text
/setup-claude Preview Mike's Claude Code setup.
```

The skill preserves unrelated configuration and only changes global Claude Code settings when explicitly asked to install or update the setup.

## What it configures

| Setting | Value |
| --- | --- |
| `model` | `claude-opus-5-5` |
| `effortLevel` | `medium` |
| `showThinkingSummaries` | `true` |
| `permissions.defaultMode` | `default` |
| `sandbox.enabled` | `true` |
| `sandbox.autoAllowBashIfSandboxed` | `true` |

Plus four subagents in `<claude-home>/agents/`: a read-only `researcher` on Sonnet 5 at `high`, workspace-writing `frontend-engineer` and backend `engineer` agents on Opus 5.5 at `high`, and a read-only `reviewer` on Opus 5.5 at `high`. The global `CLAUDE.md` block routes work to those roles and runs research, implementation, and review as separate phases.

Opus 5.5 takes the orchestrator, engineer, and reviewer seats. It performs at Fable 5.1's level on most work at two fifths of the price, which matters most for review because it runs the most often. Sonnet 5 takes the researcher seat, which reads the most tokens and makes the fewest decisions. Fable 5.1 stays available through `/model` for a hard problem.

Some Codex settings have no counterpart here. Claude Code has no session cap on concurrent subagents and no global default subagent model, and web search, web fetch, and context compaction are built in.
