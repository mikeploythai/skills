# Contributor documentation

Write documentation as normal prose, with ordinary capitalization, punctuation, and readable paragraphs. The codestyle conventions apply to source code and machine-readable configuration in code projects, including code examples in docs. They do not govern README text, feature maps, design guidance, product copy, or messages to the user. Follow the project's prose style where one exists.

Leave enough context for the next contributor to understand what is where and how to work on it. The subagent changing a feature should update its docs too. If several agents are working on the same docs, give one of them ownership so they don't overwrite each other.

## Feature map

Read the feature map before assigning work, then update it as each slice lands. Use the one the project already has. If there isn't one, create `FEATURES.md` once there's an actual feature to describe.

For each feature, explain what it does, where someone enters the flow, which code handles it, how the data moves, and how to verify it. Link to real files and checks. Include limitations that someone contributing needs to know about. Don't list planned features as if they've already been built.

Something like this is enough. These paths are examples; replace them with the project's actual paths:

| Feature | Where it starts | Where it lives | How to check it |
| --- | --- | --- | --- |
| Edit a saved view | Views screen | `src/views/` handles the form and API; `src/data/views.ts` saves the data | `tests/views.test.ts`; save and reopen a view |

## When to create `DESIGN.md`

Don't write `DESIGN.md` just because interface work has started. A first mockup, a component library, or a palette isn't an established design style yet.

There should be an actual flow or accepted design that shows the typography, colors, spacing, components, and interactions working together. The user should have accepted that direction, or the project should already have a consistent, shipped design system that the work is continuing. Don't turn unresolved choices into rules.

For an existing product, look through its screens, components, and tokens first. If it already has useful design docs, update those instead of creating another version. There's no need to ask the user to approve an established style again.

For a new product or redesign, build enough of a representative flow for the user to see how it looks and feels. If they haven't accepted the direction yet, show the preview or screenshots and ask whether this is the style they want to continue with. Point out anything that's still undecided. Passing tests, no response, or approval that a feature works doesn't mean the user has approved its design.

Until then, keep design exploration in the task or existing working notes. Keep independent work moving while waiting for feedback. Don't create a design guide just to check a box.

## What goes in `DESIGN.md`

Once the style is established, help future contributors understand it and build on it:

- How the product should look and feel, with accepted examples and the reasons behind important choices;
- Where the tokens, typography, colors, spacing, and components are defined;
- How interactions and layouts behave across screen sizes;
- Which existing components or flows to follow when adding something;
- Any exceptions or decisions that are still open.

Link to the implementation instead of copying values that will go stale. Keep this about the actual product; general interface advice belongs in the design references. Update it when an accepted design decision changes. Following an existing pattern doesn't need another round of style approval.

## How to contribute

Keep setup steps, commands, and contribution instructions in the existing README or contribution guide. Link to the feature map and design docs when they exist. Check that changed paths and commands are right before finishing the slice. Don't leave duplicate guides or placeholders for someone else to figure out.

## Source

Informed by [Vercel's explanation of design.md](https://vercel.com/blog/how-our-agents-build-on-brand-pages-with-design-md), reviewed 2026-09-10. This guide was written for Mike's way; it isn't a copy of the article or Vercel's design system. The article informed the use of concrete examples and links to implementation. The feature map and timing for `DESIGN.md` are local requirements. Agents don't need to fetch the article to follow this guide.
