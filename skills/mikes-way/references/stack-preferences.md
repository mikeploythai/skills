# stack preferences

these are the tools mike likes working with. when the user is figuring out how to build something, suggest the relevant ones and explain why you'd use them. ask before committing them to a new service, dependency, or architecture.

don't replace a working stack just to match this list, or install everything here at the start of a project. use what the app actually needs. check the current docs, compatibility, maintenance, and pricing before adding something.

## general

prefer full-stack typescript, but keep the setup proportional to what we're building:

- for a quick mockup, plain html, css, and javascript are fine. add the tailwind cli, a small server, htmx, or alpine.js if you need them.
- for a site that's mostly content, use astro. keep the content in the project unless people need a cms to edit it. sanity and self-hosted payload are the preferences there.
- for an app without ssr, start with vite and convex.
- if the app needs a traditional server, use vite with elysia or hono. elysia is the current preference. expose openapi and generate the client with heyapi's vite plugin and its tanstack query integration.
- use tanstack start or next.js app router if the app needs ssr or benefits from keeping the frontend and backend in one framework.

default new web apps and mockups to no indexing until the user says they should
be discoverable. add a `robots.txt` that disallows all crawlers, plus a
`noindex, nofollow` meta tag or `X-Robots-Tag` response header. remove them when
indexing is wanted. robots directives are not access control.

### toolchain

use [vite+](https://viteplus.dev/guide/) for the web toolchain. let it manage node.js, pnpm, vite, vitest, oxlint, and oxfmt, and use the `vp` commands. stick with pnpm unless the project already uses or requires something else.

use pnpm workspaces for a monorepo. add turborepo if you need its task dependencies, caching, or remote execution. make the dev command start the server and wait for its openapi document before starting the frontend. heyapi needs the server running to generate the client. if you're using drizzle, start drizzle studio too.

keep linting and formatting under one `check` script. add the database and ui shortcuts if those tools are in the project:

```json
{
  "scripts": {
    "check": "vp check --fix",
    "db:studio": "drizzle-kit studio",
    "ui:add": "shadcn add"
  }
}
```

use vitest for code tests. use playwright for browser tests that need to run in ci or catch a regression. for a one-off check, have the subagent(s) use the browser and try the flow.

### vite+ configuration

read [vite+ configuration](vite-plus-config.md) when setting up or changing vite+. it has the lint and format settings to start from. use the oxc integration for react compiler if the installed version supports it.

when a vite app needs local https, use `vite-plugin-mkcert`. add it as a dev dependency and add `mkcert()` to the vite plugins. use the https origin for msal react development and match it in the app's registered redirect uri.

### frontend choices

start with react, react compiler, and tanstack router. use shadcn/ui with base ui for components. use react aria when the app has major accessibility requirements.

```sh
pnpm dlx shadcn@latest init --preset b2BVlsCDD --template vite --pointer
pnpm dlx shadcn@latest init --preset b2BVlsCDD --base aria --template vite --pointer
```

when using tanstack router, prefer file-based routing. nest groups with several
routes in folders, and add `route.tsx` when the group needs a shared layout,
loader, or other parent behavior:

```text
folderA/
├── page1.$id.tsx
├── page2.tsx
└── route.tsx
folderB.onlyPage.$id.tsx
```

dot notation is fine for a nest with only one page, as in the `folderB`
example. apply the route filename lint override and project-level vscode
settings in [vite+ configuration](vite-plus-config.md).

in a shadcn project, check the installed components and the catalog before
writing a new component. use `pnpm ui:add <component>` when that shortcut
exists, or `pnpm dlx shadcn@latest add <component>`, then adapt its source.
write one from scratch only when the catalog has nothing that fits the needed
interaction.

for the rest, these are the preferences. add them as you need them:

- tanstack query for data fetching, pacer for timing and rate control, and hotkeys for keyboard shortcuts.
- tanstack table for data grids, virtual for long lists, and charts for data visualization.
- tanstack form for new forms. react hook form is fine if it's already in the project or has something the app needs.
- zod for runtime validation and t3 env for typed environment variables.
- dnd kit for drag and drop, react dropzone for file drops, react email for email templates, and tiptap for rich text.
- sonner, vaul, and cmdk when the component base does not already cover toasts, drawers, or command menus.
- jotai when react context isn't satisfactory.

prefer hugeicons or lucide for icons. use tailwind css with proper design tokens. stylex is worth considering for a custom design system built on base ui or react aria, but its stricter styling model comes at the cost of human developer experience.

use fontsource for app fonts. instrument sans and cascadia code are the current favorites. other fonts to consider are inter, space mono, jetbrains mono, space grotesk, manrope, figtree, dm mono, bricolage grotesque, mona sans, unbounded, roboto flex, google sans, and google sans flex. pick something that suits the app. don't use the same font for everything just because it's on this list.

### backend and platform choices

prefer convex for the backend. check what it and its components already do before writing auth, email, rate limiting, scheduled or durable work, presence, aggregates, migrations, or blob storage yourself.

prefer better auth. use clerk if the user wants managed auth. use resend for email, though nodemailer is fine for something small or an integration that needs it.

for a traditional server, use elysia or hono and generate the frontend client from openapi with heyapi's vite plugin. use drizzle if you need an orm. for data:

- use cloudflare d1 for sqlite, or local node.js sqlite for isolated development data. use durable objects when the app needs their coordination model.
- use neon or planetscale for postgres, after checking the cost with the user.
- use planetscale for mysql, after checking the cost with the user.
- cloudflare r2 for blobs, with uploadthing as a managed alternative.

supabase is fine too. convex is the preference.

for ai, consider tanstack ai if you're already using tanstack. it can stream over http. the vercel ai sdk is also a good option, especially if it has the provider or ui support you need. if the stream comes from a convex http action or a non-typescript server, test that the client can actually read it. use openrouter if cloudflare's ai tools aren't a better choice for the app.

prefer cloudflare for infrastructure. one account can cover hosting, domains, secrets, workers, workers ai, ai gateway, d1, durable objects, and r2. vercel is fine if it's a better choice for the app.

start with posthog for observability. use sentry if you need more help diagnosing errors. for simple analytics, self-hosted umami with postgres or one dollar stats is enough.

## mike's work environment

this part is mostly for mike's day job, where production runs on on-prem iis and microsoft sql server. there's still .net framework, with a move toward modern .net. it might help someone working in a similar setup, but don't assume every project has these constraints.

use a vite frontend and generate the openapi client with heyapi's vite plugin. a small server can use `.ashx` handlers and nswag.generation. on .net framework, prefer microsoft asp.net web api with owin if the app supports it. on modern .net, use minimal apis. build the frontend into `wwwroot` if that's where the iis app serves it from.

use msal react in the browser, microsoft.identity.web on the server, and the microsoft graph sdk for microsoft 365 integration. prefer azure key vault for secrets. the fallback is gitignored `secrets.config` for `<appSettings>` and `database.config` for connection strings.

production uses on-prem sql server, but do all data work in sqlite during development so it doesn't affect the production database. when mike is ready, have a subagent create a sql server migration based on the changes made in sqlite. account for differences between sqlite and sql server, and check the migration against a local or test sql server before handing it back for review. creating the migration doesn't mean running it against production.

prefer microsoft.data.sqlclient for sql server, and add dapper if it makes the queries or mapping easier. use system.text.json if the target framework supports what you need.

for ai, prefer microsoft semantic kernel with microsoft foundry or openrouter. stream responses over http with server-sent events if you only need server-to-client updates. you can use tanstack ai's react client if the server sends the stream format it expects.

if web forms is unavoidable, keep it close to plain web development. disable viewstate, render markup in `.aspx`, and use `.aspx.cs` only to load data or add markup through `asp:Literal` and `asp:Placeholder`. don't add other server or custom controls. use `.ashx` for api endpoints, htmx with alpine.js or jquery for browser behavior, jsdelivr for cdn dependencies, and a local tailwind cli build. if bootstrap is unavoidable, check the current bootstrap 6 prerelease before using it. set up microsoft.aspnet.friendlyurls.core.
