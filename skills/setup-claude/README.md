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
| `model` | `claude-opus-5` |
| `effortLevel` | `xhigh` |
| `showThinkingSummaries` | `true` |
| `permissions.defaultMode` | `ask` |
| `sandbox.enabled` | `true` |
| `sandbox.autoAllowBashIfSandboxed` | `true` |

Plus three subagents in `<claude-home>/agents/`: a read-only `researcher` and a workspace-writing `engineer` on Sonnet 5 at `max` effort, and a read-only `reviewer` on Opus 5 at `xhigh`.

Some Codex settings have no counterpart here. Claude Code has no session cap on concurrent subagents and no global default subagent model, and web search, web fetch, and context compaction are built in.
