# Mike's way

My rules and preferences for building software, tailored for agentic workflows.

See [SKILL.md](SKILL.md) for the full rules.

## Installation

With Node.js and npm available, run:

```sh
npx skills add mikeploythai/skills --skill mikes-way
```

Follow the prompts to choose the agent and installation scope. See the [Skills CLI docs](https://skills.sh/docs/cli) for other options.

For a manual install, copy `skills/mikes-way/` into your agent's skill directory as `mikes-way`. Keep its `SKILL.md` and `references/` together; the skill reads those references as needed. Use the skill directory documented by your agent.

## Usage

Ask your agent to use it with a concrete task:

```text
$mikes-way Add saved views to this app.
```

The skill asks the agent to delegate work, reuse existing capabilities, check what a change could break, and keep feature and design documentation current. Existing repo conventions take priority over its code defaults.

When choosing a stack, the agent reads [stack preferences](references/stack-preferences.md) for the tools Mike likes working with. It also includes [frontend toolchain guidance](references/frontend-toolchain.md) for Vite+, linting, styling integrations, and editor settings. These are suggestions, so a working project doesn't need to change stacks to use the skill.

The feature map grows as features land. `DESIGN.md` waits until there's an established design direction, so an early mockup doesn't accidentally become the standard.

It also tells the agent to commit completed slices and open issues worth tracking. Pushing and deploying still need the user's consent.

## Optional companions

These aren't required, but heavily encouraged to install:

- [Ponytail](https://github.com/DietrichGebert/ponytail) for simpler code and less unnecessary building.
- [Cloudflare's Kumo design skill](https://github.com/cloudflare/kumo/tree/main/skills/kumo-design) for interface guidance. Using it doesn't mean adopting the Kumo library.
- [Jakub Krehel's skills](https://github.com/jakubkrehel/skills) for focused interface design and review.
- [Emil Kowalski's skills](https://github.com/emilkowalski/skills) for interface polish, component design, and animation decisions.
- [CodeGraph](https://github.com/colbymchenry/codegraph) for finding symbols and tracing call paths when a usable index is available. Otherwise the agent uses `rg` and reads the files.

Missing companions shouldn't hold up tasks. The agent can recommend one when it would help once, but won't install it without approval.

See [design task routing](references/interface-design.md#choosing-skills-by-task) for which skills handle prototyping, implementation, reviews, motion planning, testing, and terminology.

## Credits

The bundled references include Lauren Tan's Unslop and a standalone adaptation of Blast radius. See [upstream notices](references/upstream-notices.md) for sources, changes, and license text.
