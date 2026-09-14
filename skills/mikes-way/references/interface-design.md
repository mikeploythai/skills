# Interface design

Give the interface some personality. Color, typography, imagery, and motion are all encouraged, as long as the product still feels like something people can trust to do real work. It shouldn't feel like a toy. Keep frequent actions easy to find and use; don't fill the screen with decorative cards or so much spacing that useful information gets pushed out.

Avoid AI-generated interface tells in shipped UI and mockups, especially hi-fi mockups. Don't add eyebrow labels, headings that repeat nearby copy, explanations for self-evident controls, fake metrics, decorative cards, or visual effects without a product reason. Every element should help someone act, understand the current state, or recover from a problem.

Look at the project's existing screens, components, client-side libraries, tokens, and design docs before changing things. Build on the style and features that are already there. If a style hasn't been established yet, follow [contributor documentation](contributor-docs.md) before writing it down as the standard.

Think through the states people will actually run into: loading, empty, error, disabled, and success. Keep labels clear, text readable, and controls usable with a keyboard, including a visible focus state. Use the browser when available to try the flow at relevant screen sizes with realistic content. Be clear about anything that couldn't be checked.

When relevant, stress the real interface with dense lists, long and unbroken text, multilingual content, narrow widths, and short viewports. Confirm actions that discard work, return focus after dialogs and validation errors, and respect reduced-motion settings. Use shared components without leaving duplicates that shadow the customized version.

## Evolving the design system

Shared components own reusable appearance; callers arrange them within a page. This applies to CSS, Tailwind, StyleX, and native component APIs.

- Reuse named variants and sizes. Put a new reusable treatment in the owning component instead of repeating local overrides. Use semantic tokens.
- Define caller-controlled styling explicitly through existing props, style APIs, or conventions. A container might accept spacing, a title typography, and an avatar size. Choose each boundary deliberately.
- Keep exceptions narrow. Permission to adjust spacing does not justify raw colors or arbitrary appearance values. Explain the approved alternative when rejecting a change.
- Review new tokens, variants, styling permissions, and suppressions as design decisions. Record accepted changes in existing design docs, following [contributor documentation](contributor-docs.md). Automated checks enforce policy; they cannot decide whether a treatment belongs in the system.

Adapted from [shadcn's design-system guidance](https://github.com/shadcn-ui/lint/blob/main/docs/design-systems.md). These principles do not require its linter. For Tailwind enforcement, use [frontend toolchain guidance](frontend-toolchain.md#tailwind-design-system-linting).

## Companion skills

These can help with design taste. Follow the guidance on optional companions in [Mike's way](../SKILL.md):

- [Cloudflare's Kumo design skill](https://github.com/cloudflare/kumo/tree/main/skills/kumo-design) has useful guidance even when the project doesn't use Kumo. Fit the visual choices to the project, and only use its component examples when those components are actually available. The skill doesn't require adopting the Kumo UI library.
- [Jakub Krehel's skills](https://github.com/jakubkrehel/skills) cover different parts of interface design and review. Use the ones relevant to the work.
- [Emil Kowalski's skills](https://github.com/emilkowalski/skills) cover interface polish, component design, and deliberate motion.

## Choosing skills by task

Choose the skill that matches the requested task and scope, then read it before acting. Load its required companions and only the references relevant to that work. Pass those choices to the agent doing the work. Use the focused domain guidance below during ordinary implementation; a full audit is a separate task.

| Work | Use | When and result |
| --- | --- | --- |
| General guidelines | `emil-design-eng` | Interface polish, component craft, and motion decisions that fit the product. |
| Interaction guidelines | `apple-design` | Gestures, physical and interruptible motion, springs, depth, materials, and typography. Adapt the principles to the product's established style. |
| Interface guidelines | `kumo-design` | Interface conventions, dashboard usability, and design tokens. Its guidelines apply without the Kumo UI library; use component examples only when those components are available. |
| Visual prototyping | `variant` | Compare versions of a component in its real page context, varying one primary axis such as layout, density, emphasis, typography, or voice. |
| Broader prototyping | `prototype` | Compare distinct layout, motion, or interaction directions in an isolated prototype surface with realistic surrounding context. |
| Web motion implementation | `animate` | Build an animation or implement a requested motion fix. Writes code for timing, curves, properties, interruption, exits, and reduced motion. |
| Native motion implementation | `animate-expo` | Build or fix React Native and Expo animations, gestures, sheets, transitions, and haptics. Writes code for the native stack. |
| Comprehensive interface audit | `better-interface` | Review an existing screen or flow across accessibility, layout, writing, typography, color, and UI polish. Coordinates the domain owners below into one report. |
| Focused interface audit | The relevant `better-*` skill below | Review one named domain without starting a comprehensive interface audit. |
| Interface change review | `interface-review` | Review a branch, pull request, commit range, or uncommitted changes. Resolves the diff, affected surfaces, and finding statuses, then hands them to `better-interface`, which routes the domain owners. |
| Motion change review | `review-animations` | Review one animation or motion diff, with concrete findings and proposed fixes. |
| Motion discovery | `find-animation-opportunities` | Identify where motion would help and where it should be avoided. Proposes opportunities; does not implement them. |
| Motion audit and planning | `improve-animations` | Audit existing motion across a codebase and write selected implementation plans in `plans/`. Hand plans to `animate` or `animate-expo` for implementation. Its explicit `execute` mode dispatches an implementing subagent. |
| Reverse engineering | `explain-interface` | Explain how an existing interface or visual effect is built and what each layer contributes. A screenshot supports reconstruction, not verified implementation, motion, or timing. Gives no audit verdict. |
| Component stress testing | `break` | Stress one existing component with fixture props and states it can actually reach. Use difficult content and viewport sizes on a temporary page; report observed breaks without fixes or a verdict. Leave the page available for manual review until cleanup is requested. |
| Motion terminology | `animation-vocabulary` | Turn a described effect into the right term for discussion or prompting. This is a glossary, not an implementation workflow. |

### Focused domain guidance and audits

Use these skills both for guidance while building and for a requested audit of their domain. `better-interface` owns consolidation when the task spans the whole interface; `interface-review` owns change scope when the task names a diff.

| Focus | Use |
| --- | --- |
| Semantics, accessible names, keyboard use, focus, hit-area requirements, and reduced motion | `better-accessibility` |
| Grouping, alignment, reading order, density, responsive layout, and disclosure | `better-layout` |
| Labels, messages, terminology, and product copy | `better-writing` |
| Type scale, spacing, wrapping, truncation, and font details | `better-typography` |
| Palettes, semantic tokens, contrast, themes, and color conversions | `better-colors` |
| Radii, optical alignment, surface depth, contextual icons, and press feedback | `better-ui` |

### Selection and execution boundaries

- Honor a skill the user names, and preserve each skill's invocation policy. The reviewed skills and Codex metadata make `prototype`, `variant`, `review-animations`, `explain-interface`, and `break` explicit-only. The table describes which fits; it does not enable automatic invocation. Both prototyping skills present working alternatives and wait for the user's choice before promoting a winner.
- Reviews, audits, explanations, and motion planning keep application source read-only by default. Planning and testing may write their working artifacts. Implement findings only when implementation is requested, following the selected skill's workflow. Follow `improve-animations`' finding-selection step before writing plans. It does not apply fixes itself; its explicit `execute` mode hands implementation to a subagent.
- Keep stress fixtures and prototype harnesses out of shipped production entry points and live writes. Follow `break`'s cleanup rules for testing artifacts.
- If a companion is missing, follow the optional-companion guidance above. If the selected skill requires a missing owner or dependency, follow its stated fallback or stopping rule and report the coverage gap.
