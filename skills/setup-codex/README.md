# setup codex

install, update, preview, or explain mikeploythai's opinionated codex
multi-agent setup.

see [SKILL.md](SKILL.md) for the installation workflow and
[configuration](references/configuration.md) for the settings and agent
definitions.

this setup is designed to pair with [mike's way](../mikes-way/README.md),
which supplies the primary agent's engineering-manager rules.

## installation

with node.js and npm available, run:

```sh
npx skills add mikeploythai/skills --skill setup-codex
```

follow the prompts to choose the codex installation scope. see the
[skills cli docs](https://skills.sh/docs/cli) for other options.

## usage

explicitly ask your agent to preview, install, or update the setup:

```text
$setup-codex preview mike's codex setup.
```

the skill preserves unrelated configuration and only changes global codex
settings when explicitly asked to install or update the setup.
