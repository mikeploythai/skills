# Setup Claude

Install, update, preview, or explain mikeploythai's opinionated Claude Code multi-agent setup.

See [SKILL.md](SKILL.md) for the installation workflow and [configuration](references/configuration.md) for the settings and subagent definitions.

This setup pairs with [Mike's way](../mikes-way/README.md), which supplies the primary agent's engineering-manager rules. It is the Claude Code counterpart to [Setup Codex](../setup-codex/README.md).

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
| `model` | `claude-fable-5-1` |
| `effortLevel` | `medium` |
| `showThinkingSummaries` | `true` |
| `permissions.defaultMode` | `default` |
| `sandbox.enabled` | `true` |
| `sandbox.autoAllowBashIfSandboxed` | `true` |

Plus three subagents in `<claude-home>/agents/`: a read-only `researcher` at `high` and a workspace-writing `engineer` at `xhigh`, both on Sonnet 5, and a read-only `reviewer` on Fable 5.1 at `high`.

Fable 5.1 takes the orchestrator and reviewer seats, where judgment pays and output volume is low. Sonnet 5 takes the researcher and engineer seats, which read and write the most tokens. Fable 5.1 also requires 30-day data retention; the [configuration reference](references/configuration.md) names the Opus 5 substitution for organizations that cannot meet that.

Some Codex settings have no counterpart here. Claude Code has no session cap on concurrent subagents and no global default subagent model, and web search, web fetch, and context compaction are built in.
