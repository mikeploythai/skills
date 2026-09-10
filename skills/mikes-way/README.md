# mike's way

my rules and preferences for building software, tailored for agentic workflows.

see [SKILL.md](SKILL.md) for the full rules.

## installation

with node.js and npm available, run:

```sh
npx skills add mikeploythai/skills --skill mikes-way
```

follow the prompts to choose the agent and installation scope. see the [skills cli docs](https://skills.sh/docs/cli) for other options.

for a manual install, copy `skills/mikes-way/` into your agent's skill directory as `mikes-way`. keep its `SKILL.md` and `references/` together; the skill reads those references as needed. use the skill directory documented by your agent.

## usage

ask your agent to use it with a concrete task:

```text
$mikes-way add saved views to this app.
```

the skill asks the agent to delegate work, reuse existing capabilities, check what a change could break, and keep feature and design documentation current. existing repo conventions take priority over its code defaults.

the feature map grows as features land. `DESIGN.md` waits until there's an established design direction, so an early mockup doesn't accidentally become the standard.

it also tells the agent to commit completed slices and open issues worth tracking. pushing and deploying still need the user's consent.

## optional companions

these aren't required, but heavily encouraged to install:

- [ponytail](https://github.com/DietrichGebert/ponytail) for simpler code and less unnecessary building.
- [cloudflare's kumo design skill](https://github.com/cloudflare/kumo/tree/main/skills/kumo-design) for interface guidance. using it doesn't mean adopting the kumo library.
- [jakub krehel's skills](https://github.com/jakubkrehel/skills) for focused interface design and review.
- [codegraph](https://github.com/colbymchenry/codegraph) for finding symbols and tracing call paths when a usable index is available. otherwise the agent uses `rg` and reads the files.

missing companions shouldn't hold up tasks. the agent can recommend one when it would help once, but won't install it without approval.

## credits

the bundled references include lauren tan's unslop and a standalone adaptation of blast radius. see [upstream notices](references/upstream-notices.md) for sources, changes, and license text.
