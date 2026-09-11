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
