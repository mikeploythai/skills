# contributor documentation

leave enough context for the next contributor to understand what is where and how to work on it. the subagent changing a feature should update its docs too. if several agents are working on the same docs, give one of them ownership so they don't overwrite each other.

## feature map

read the feature map before assigning work, then update it as each slice lands. use the one the project already has. if there isn't one, create `FEATURES.md` once there's an actual feature to describe.

for each feature, explain what it does, where someone enters the flow, which code handles it, how the data moves, and how to verify it. link to real files and checks. include limitations that someone contributing needs to know about. don't list planned features as if they've already been built.

something like this is enough. these paths are examples; replace them with the project's actual paths:

| feature | where it starts | where it lives | how to check it |
| --- | --- | --- | --- |
| edit a saved view | views screen | `src/views/` handles the form and API; `src/data/views.ts` saves the data | `tests/views.test.ts`; save and reopen a view |

## when to create `DESIGN.md`

don't write `DESIGN.md` just because interface work has started. a first mockup, a component library, or a palette isn't an established design style yet.

there should be an actual flow or accepted design that shows the typography, colors, spacing, components, and interactions working together. the user should have accepted that direction, or the project should already have a consistent, shipped design system that the work is continuing. don't turn unresolved choices into rules.

for an existing product, look through its screens, components, and tokens first. if it already has useful design docs, update those instead of creating another version. there's no need to ask the user to approve an established style again.

for a new product or redesign, build enough of a representative flow for the user to see how it looks and feels. if they haven't accepted the direction yet, show the preview or screenshots and ask whether this is the style they want to continue with. point out anything that's still undecided. passing tests, no response, or approval that a feature works doesn't mean the user has approved its design.

until then, keep design exploration in the task or existing working notes. keep independent work moving while waiting for feedback. don't create a design guide just to check a box.

## what goes in `DESIGN.md`

once the style is established, help future contributors understand it and build on it:

- how the product should look and feel, with accepted examples and the reasons behind important choices;
- where the tokens, typography, colors, spacing, and components are defined;
- how interactions and layouts behave across screen sizes;
- which existing components or flows to follow when adding something;
- any exceptions or decisions that are still open.

link to the implementation instead of copying values that will go stale. keep this about the actual product; general interface advice belongs in the design references. update it when an accepted design decision changes. following an existing pattern doesn't need another round of style approval.

## how to contribute

keep setup steps, commands, and contribution instructions in the existing README or contribution guide. link to the feature map and design docs when they exist. check that changed paths and commands are right before finishing the slice. don't leave duplicate guides or placeholders for someone else to figure out.

## source

informed by [vercel's explanation of design.md](https://vercel.com/blog/how-our-agents-build-on-brand-pages-with-design-md), reviewed 2026-09-10. this guide was written for mike's way; it isn't a copy of the article or vercel's design system. the article informed the use of concrete examples and links to implementation. the feature map and timing for `DESIGN.md` are local requirements. agents don't need to fetch the article to follow this guide.
