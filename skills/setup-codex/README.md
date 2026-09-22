# Setup Codex

Install, update, preview, or explain mikeploythai's opinionated Codex multi-agent setup.

See [SKILL.md](SKILL.md) for the installation workflow and [configuration](references/configuration.md) for the settings and agent definitions.

The setup adds Mike's way and CodeGraph guidance to the global `AGENTS.md`. Install [Mike's way](../mikes-way/README.md) for its default instruction to work. CodeGraph is optional.

## Installation

With Node.js and npm available, run:

```sh
npx skills add mikeploythai/skills --skill setup-codex
```

Follow the prompts to choose the Codex installation scope. See the [Skills CLI docs](https://skills.sh/docs/cli) for other options.

## Usage

Explicitly ask your agent to preview, install, or update the setup:

```text
$setup-codex Preview Mike's Codex setup.
```

The skill preserves unrelated configuration and only changes global Codex settings when explicitly asked to install or update the setup.
