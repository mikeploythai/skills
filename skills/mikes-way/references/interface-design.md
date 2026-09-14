# interface design

give the interface some personality. color, typography, imagery, and motion are all encouraged, as long as the product still feels like something people can trust to do real work. it shouldn't feel like a toy. keep frequent actions easy to find and use; don't fill the screen with decorative cards or so much spacing that useful information gets pushed out.

avoid ai-generated interface tells in shipped ui and mockups, especially hi-fi
mockups. don't add eyebrow labels, headings that repeat nearby copy,
explanations for self-evident controls, fake metrics, decorative cards, or
visual effects without a product reason. every element should help someone act,
understand the current state, or recover from a problem.

look at the project's existing screens, components, client-side libraries, tokens, and design docs before changing things. build on the style and features that are already there. if a style hasn't been established yet, follow [contributor documentation](contributor-docs.md) before writing it down as the standard.

think through the states people will actually run into: loading, empty, error, disabled, and success. keep labels clear, text readable, and controls usable with a keyboard, including a visible focus state. use the browser when available to try the flow at relevant screen sizes with realistic content. be clear about anything that couldn't be checked.

when relevant, stress the real interface with dense lists, long and unbroken
text, multilingual content, narrow widths, and short viewports. confirm actions
that discard work, return focus after dialogs and validation errors, and respect
reduced-motion settings. use shared components without leaving duplicates that
shadow the customized version.

## companion skills

these can help with design taste. follow the guidance on optional companions in [mike's way](../SKILL.md):

- [cloudflare's kumo design skill](https://github.com/cloudflare/kumo/tree/main/skills/kumo-design) has useful guidance even when the project doesn't use kumo. fit the visual choices to the project, and only use its component examples when those components are actually available. the skill doesn't require adopting the kumo ui library.
- [jakub krehel's skills](https://github.com/jakubkrehel/skills) cover different parts of interface design and review. use the ones relevant to the work.
- [emil kowalski's skills](https://github.com/emilkowalski/skills) cover interface
  polish, component design, and deliberate motion.

## choosing skills by task

choose the skill that matches the requested task and scope, then read it before
acting. load its required companions and only the references relevant to that
work. pass those choices to the agent doing the work. use the focused domain
guidance below during ordinary implementation; a full audit is a separate task.

| work | use | when and result |
| --- | --- | --- |
| general guidelines | `emil-design-eng` | interface polish, component craft, and motion decisions that fit the product. |
| interaction guidelines | `apple-design` | gestures, physical and interruptible motion, springs, depth, materials, and typography. adapt the principles to the product's established style. |
| interface guidelines | `kumo-design` | interface conventions, dashboard usability, and design tokens. its guidelines apply without the kumo ui library; use component examples only when those components are available. |
| visual prototyping | `variant` | compare versions of a component in its real page context, varying one primary axis such as layout, density, emphasis, typography, or voice. |
| broader prototyping | `prototype` | compare distinct layout, motion, or interaction directions in an isolated prototype surface with realistic surrounding context. |
| web motion implementation | `animate` | build an animation or implement a requested motion fix. writes code for timing, curves, properties, interruption, exits, and reduced motion. |
| native motion implementation | `animate-expo` | build or fix react native and expo animations, gestures, sheets, transitions, and haptics. writes code for the native stack. |
| comprehensive interface audit | `better-interface` | review an existing screen or flow across accessibility, layout, writing, typography, color, and ui polish. coordinates the domain owners below into one report. |
| focused interface audit | the relevant `better-*` skill below | review one named domain without starting a comprehensive interface audit. |
| interface change review | `interface-review` | review a branch, pull request, commit range, or uncommitted changes. resolves the diff, affected surfaces, and finding statuses, then hands them to `better-interface`, which routes the domain owners. |
| motion change review | `review-animations` | review one animation or motion diff, with concrete findings and proposed fixes. |
| motion discovery | `find-animation-opportunities` | identify where motion would help and where it should be avoided. proposes opportunities; does not implement them. |
| motion audit and planning | `improve-animations` | audit existing motion across a codebase and write selected implementation plans in `plans/`. hand plans to `animate` or `animate-expo` for implementation. its explicit `execute` mode dispatches an implementing subagent. |
| reverse engineering | `explain-interface` | explain how an existing interface or visual effect is built and what each layer contributes. a screenshot supports reconstruction, not verified implementation, motion, or timing. gives no audit verdict. |
| component stress testing | `break` | stress one existing component with fixture props and states it can actually reach. use difficult content and viewport sizes on a temporary page; report observed breaks without fixes or a verdict. leave the page available for manual review until cleanup is requested. |
| motion terminology | `animation-vocabulary` | turn a described effect into the right term for discussion or prompting. this is a glossary, not an implementation workflow. |

### focused domain guidance and audits

use these skills both for guidance while building and for a requested audit of
their domain. `better-interface` owns consolidation when the task spans the
whole interface; `interface-review` owns change scope when the task names a diff.

| focus | use |
| --- | --- |
| semantics, accessible names, keyboard use, focus, hit-area requirements, and reduced motion | `better-accessibility` |
| grouping, alignment, reading order, density, responsive layout, and disclosure | `better-layout` |
| labels, messages, terminology, and product copy | `better-writing` |
| type scale, spacing, wrapping, truncation, and font details | `better-typography` |
| palettes, semantic tokens, contrast, themes, and color conversions | `better-colors` |
| radii, optical alignment, surface depth, contextual icons, and press feedback | `better-ui` |

### selection and execution boundaries

- honor a skill the user names, and preserve each skill's invocation policy.
  the reviewed skills and codex metadata make `prototype`, `variant`,
  `review-animations`, `explain-interface`, and `break` explicit-only. the table
  describes which fits; it does not enable automatic invocation. both prototyping
  skills present working alternatives and wait for the user's choice before
  promoting a winner.
- reviews, audits, explanations, and motion planning keep application source
  read-only by default. planning and testing may write their working artifacts.
  implement findings only when implementation is requested, following the
  selected skill's workflow. follow `improve-animations`' finding-selection step
  before writing plans. it does not apply fixes itself; its explicit `execute`
  mode hands implementation to a subagent.
- keep stress fixtures and prototype harnesses out of shipped production entry
  points and live writes. follow `break`'s cleanup rules for testing artifacts.
- if a companion is missing, follow the optional-companion guidance above. if
  the selected skill requires a missing owner or dependency, follow its stated
  fallback or stopping rule and report the coverage gap.
