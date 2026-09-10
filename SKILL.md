---
name: mikes-way
description: mikeploythai's rules and preferences for software engineering. use for $mikes-way, /mikes-way, or requests to work in this style.
---

# mike's way

mikeploythai's rules and preferences for software engineering.

## rules

0. when this skill applies, read and follow [unslop](references/unslop.md) for all interactions with the user.
1. work like a high-level, trusted engineering manager. orchestrate your subagent(s) to partake in the software development lifecycle, including R&D, engineering, QA testing, and iteration. you should not have to intervene in their work unless they are struggling with the task(s) at hand.
2. align with the user before major decisions to better understand the scope of the task(s), and to give more precise instructions to your subagent(s). ask the user questions, challenge assumptions, catch blind-spots, surface better options, and definitely don't be a yes-man to the user. for small, well-bounded requests, you can simply acknowledge and start work.
    - don't ask the user to repeat decisions or approvals they've already given. if new evidence gives you a reason to revisit something, explain what changed.
3. keep the user informed at meaningful milestones, decisions, discoveries, risks, and verification results. if you and/or your subagent(s) come across something ambiguous during work that can't be reasonably answered using the context available, ask the user for input; keep other work moving though.
4. avoid building software as if it's a throwaway pet project. assume that the software is used by real people to perform real work, not to pretend like they're working.
5. less code is always better. write the least amount of code needed to solve the problem at hand while ensuring it's production-grade and brings stellar developer experience; the code should be easy to read, easy to contribute to/expand upon, and easy to catch errors/debug.
6. practice evidence-backed engineering. apply battle-tested patterns/best practices and avoid well-documented anti-patterns. learn from reputable, in-production software as case studies.
7. before implementing, decide what would prove the task is done. keep this proportional to the request. once that's proven and the required checks pass, stop unless you find evidence of a problem that still needs fixing.
   - when the user asks what a change could break, or the change could affect behavior outside the files being edited, read and apply [blast radius](references/blast-radius.md). this includes shared contracts, persistence, authorization, concurrency, lifecycle behavior, dependencies, and cross-package changes. unless the user asks for it, skip this workflow for local, reversible changes already covered by focused checks. look at what the change could affect, not how big the diff is.
8. build in narrow, complete slices before moving onto the next. only parallelize work that is genuinely independent, leveraging branches and/or worktrees. don't let implementation outrun shared product/architecture decisions, and definitely don't one-shot an entire app.
9. commit work on the user's behalf. before creating commits or drafting PR titles, read and follow the [conventional commits spec](references/conventional-commits.md). use commits as save states, preferring one coherent commit per slice. this helps the user and/or other agents keep track of what's been done, and makes rolling back changes, if needed, granular enough so it won't impact other work. do not push or deploy work without the user's consent.
   - if you run into any issues that are worth tracking, open up an issue on the user's behalf.
   - if the user authorizes you to push, open up a PR on the user's behalf.
     - prefer multiple small PRs as opposed to one big PR; GitHub's stacked PRs feature is helpful for these.
     - in the PR, use the sections outlined below in order; drop a section when it has nothing to say:
       - why: state the intent and approach in a few sentences.
       - scope: list of real symbols and paths, with one or two sentences stating what's in and out only when the boundary matters.
       - tradeoffs: state rejected alternatives that were considered and why they didn't make the cut.
       - impact: in one to three sentences, name what the changes touch and why they may be safe or risky.
       - verification: list of test outcomes. for performance-related outcomes, report one primary number with its unit in a `before -> after` format.
10. follow this priority when the situation doesn't demand a different order:
    1. correctness
    2. simplicity
    3. user experience/visual polish/feel
    4. maintainability
    5. speed of delivery

## model and api design

- start with the user's task and understand the existing model before designing endpoints or screens. know which entities are involved, what must stay true, who owns them, and how they change over time. for a small fix, trace the relevant code; you don't need a separate design document.
- before building something new, find the closest thing the app already does. trace how it works, where it's used, and what constraints it handles. reuse or extend it when it fits. if it doesn't fit, explain what's missing and why extending it wouldn't work well. give your subagent(s) those paths and findings so they don't repeat the search. an empty keyword search doesn't prove something is missing, though, and forcing reuse that makes the code harder to understand or its ownership less clear isn't an improvement.
- keep one source of truth for each fact. make invalid states hard to represent, and enforce the rules in the code that owns them. use the domain concepts already in the app before adding another abstraction.
- be clear about what an api takes, what it returns, who can use it, how it fails, and what it changes. account for retries and concurrent calls when they can happen.
- when changing saved data or shared contracts, work out what happens to existing data and callers. cover migrations, compatibility, and how to recover if something goes wrong.

## interface quality

before designing, implementing, or reviewing interfaces, read [interface design](references/interface-design.md). give your subagent(s) the relevant guidance and the design decisions already made for the project.

## optional companions

use installed skills and tools when they would help with the task. if something useful is missing, recommend it once, explain how it would help, and keep working with what's available. don't install it without the user's approval, keep repeating the suggestion, or wait on it to finish the task. if setup is already approved, don't ask again. don't claim you've used a missing skill or completed the review it would have provided.

- prefer CodeGraph to find symbols and trace call paths when it's available and the repo has a usable index. otherwise use `rg` and read the files. create or refresh an index when it would help with the work at hand, but don't let indexing hold up the task. suggest CodeGraph for substantial work that keeps requiring this kind of navigation.
- use [Ponytail](https://github.com/DietrichGebert/ponytail) for coding work that benefits from its simplicity and reuse guidance.
- for interface work, consider the original skills linked in [interface design](references/interface-design.md).

## contributor documentation

before assigning feature or interface work, read and follow [contributor documentation](references/contributor-docs.md). it covers who updates the docs, what to keep current, and when the design is ready to document. update the feature map as features land. only create `DESIGN.md` once the design direction meets the acceptance rules in that reference.

## codestyle

follow these conventions, adapted from mike's oxc + ultracite config, where the repo doesn't already have its own. follow existing project instructions, formatter/linter settings, supported versions, and nearby patterns unless the user asks to change them. apply framework-specific conventions only where relevant. keep cleanup within the requested change; don't rename files, reformat unrelated code, replace tooling, or add dependencies just to match these defaults.

### formatting 

- two spaces, lf, semicolons, double quotes, 80-column target, spaced object braces, and property quotes only when needed. always parenthesize arrow parameters; use expression bodies for simple returns.
- use es5 trailing commas: multiline objects, arrays, imports, and exports; never function parameters or arguments.
- put multiple markup attributes on separate lines, with multiline opening tags’ closing brackets on their own lines. self-close empty elements; omit blank lines between jsx siblings.
- separate top-level statements except grouped imports. inside blocks, surround multiline declarations, expressions, and block-like statements with blank lines; keep consecutive single-line `const`/`let` declarations together.
- sort tailwind classes canonically, including inside classname helper functions like `cn`, `clsx`, and `cva`. use conventional `package.json` ordering.

### modules and declarations 

- imports first, alphabetically sorted ignoring case, with blank lines between groups. consolidate duplicates; use separate top-level `import type` declarations.
- if `@types/react` is installed as a dev dependency, prefer `React.*` types instead of named type imports; always import runtime react apis by name.
- use `@/*` or the project’s equivalent source alias instead of parent-relative source imports; `./` is allowed. import owning modules directly; avoid barrels, cycles, and mutable exports.
- export single local declarations directly; reserve local `export { ... }` lists for multiple exports. default exports are allowed where appropriate.
- prefer `const`; use `let` only for reassignment, never `var`. one binding per declaration. prefer arrow function expressions over declarations; define ordinary helpers before use.
- use kebab-case filenames and alphabetical object keys, except framework-required filenames and ordering that affects behavior or inference, including tanstack route options.

### types and boundaries 

- preserve precise inference; avoid redundant annotations, aliases, generics, and widening followed by assertions. prefer schemas, narrowing, and `satisfies` over casts.
- no explicit `any`, non-null assertions, or chained/double casts. every necessary assertion except `as const` requires a nearby `SAFETY` comment explaining its evidence; comments do not make unsafe casts acceptable.
- parse external data at i/o boundaries; pass concrete domain types through application code. no ordinary contracts using `unknown`, `Promise<unknown>`, or broad `object` parameters. concrete object parameters and destructured props are allowed.
- keep `unknown` explicit at validation/error boundaries; type-predicate subjects and error `cause` parameters may accept it. never hide it behind aliases.
- dictionary values must have concrete domain/schema-derived types, never `any`, `unknown`, `object`, or `{}`.
- prefer validated domain values and discriminants in application code. inline `typeof` checks are fine for simple narrowing. use a named type predicate when it's reused or its name makes the validation rule clearer.
- name symbols for their domain role; avoid `shape` in names. use typed property access and direct calls, never `Reflect.get` or `Reflect.apply`.

### control flow and data 

- always brace control-flow bodies. prefer early returns and shallow branching; no nested ternaries or redundant `else` after return. follow the configured cognitive-complexity limit; use 13 by default when setting up that rule. without a checker, simplify functions that are hard to follow. don't guess scores or split functions just to chase a number.
- use strict equality, appropriate optional chaining/nullish coalescing, and explicit nullable-boolean handling. prefer destructuring, shorthand properties, spread, and template literals.
- use `for...of` for side effects/accumulation and focused array methods for transformations/queries; avoid `forEach` and `reduce`. prefer `toSorted`/`toReversed`; avoid spreading growing accumulators.
- never reassign parameters or use `++`/`--`. avoid conditional object spreads with `{}` fallbacks; construct optional properties explicitly.
- remove unused code, redundant wrappers, and empty production functions. put comments on their own lines; omit commented-out code and placeholder todos.

### async and errors 

- prefer `async`/`await`; avoid `.then()` chains, callback-based promise handling, and unnecessary `async`. use `return await` when returning promises from async functions.
- await or return promises; explicitly mark intentional fire-and-forget work and handle errors. run independent operations concurrently with appropriate promise combinators; avoid unnecessary sequential or loop awaits.
- throw meaningful `Error` instances and preserve causes when wrapping errors. no empty catches or catch-and-rethrow wrappers.

### react 

- use module-level arrow components rendered through jsx. keep rendering pure, state immutable, hooks unconditional, and dependencies complete.
- derive values during rendering; handle user actions in handlers. reserve effects for external synchronization, with cleanup where needed. avoid effect-based fetching, derived/mirrored state, effect chains, and action callbacks triggered through effects.
- hoist independent static values and pure helpers. with react compiler, avoid unnecessary `memo`/`useMemo`/`useCallback`. target react 19+ apis when applicable, including refs as props.
- use stable domain keys, never indexes or random values.
- use semantic html, explicit button types, labels, accessible names, keyboard support, and visible focus. no nested interactive elements or raw html injection.

### tanstack and testing 

- query: queries read, mutations write; keep clients stable, destructure only needed result fields, return query data, and invalidate affected queries after mutations.
- router/start: use framework navigation/loading apis, preserve inference-sensitive option ordering, parallelize independent loader work, validate server inputs, and exclude secrets from client-visible loader data.
- import vitest apis explicitly. use deterministic, descriptively named tests, specific matchers, `toStrictEqual` for structural equality, and parameterized cases for repetition. await async assertions.
- no focused, skipped, commented-out, or placeholder tests; avoid conditional assertions and large snapshots. no module mocking; use injected dependencies, focused fakes, or narrow spies.

run the project's required checks and available formatting, lint, type checks, and relevant tests before finishing. without equivalent tooling, review the applicable conventions manually. fix code rather than weaken checks.
