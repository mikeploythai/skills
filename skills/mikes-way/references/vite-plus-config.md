# Vite+ configuration

This is Mike's starting linting and formatting configuration for `vite.config.ts`. Keep it when the installed versions support it and the repo has not chosen different rules. Remove framework-specific presets, JS plugins, and overrides when they do not apply. Include `@shadcn/lint` only for compatible Tailwind v4 projects, as described below.

```ts
import ultracite from "ultracite/oxfmt";
import antiSlop from "ultracite/oxlint/anti-slop";
import core from "ultracite/oxlint/core";
import jsPlugins, { jsPluginSettings } from "ultracite/oxlint/js-plugins";
import reactLint from "ultracite/oxlint/react";
// TanStack projects only, including the matching presets below.
import tanstack from "ultracite/oxlint/tanstack";
import tanstackJsPlugins from "ultracite/oxlint/tanstack/js-plugins";
import vitest from "ultracite/oxlint/vitest";
import { defineConfig } from "vite-plus";

export default defineConfig({
  fmt: {
    ...ultracite,
    singleAttributePerLine: true,
  },
  lint: {
    extends: [
      core,
      reactLint,
      // TanStack projects only.
      tanstack,
      vitest,
      jsPlugins,
      // TanStack projects only.
      tanstackJsPlugins,
      antiSlop,
    ],
    ignorePatterns: core.ignorePatterns,
    jsPlugins: [
      ...(jsPlugins.jsPlugins ?? []),
      // Tailwind v4 projects only.
      "@shadcn/lint",
      {
        name: "vite-plus",
        specifier: "vite-plus/oxlint-plugin",
      },
      {
        name: "stylistic",
        specifier: "@stylistic/eslint-plugin",
      },
      {
        name: "eslint-js",
        specifier: "oxlint-plugin-eslint",
      },
    ],
    options: {
      typeAware: true,
      typeCheck: true,
    },
    overrides: [
      {
        files: ["src/**"],
        rules: {
          "no-restricted-imports": [
            "error",
            {
              patterns: [
                {
                  message: "Use the @/* alias for imports outside the parent folder.",
                  regex: "^\\.\\./\\.\\./",
                },
              ],
            },
          ],
        },
      },
      // TanStack Router projects only.
      {
        files: ["src/routes/**/*.tsx"],
        rules: {
          "github/filenames-match-regex": "off",
        },
      },
      // Tailwind v4 projects only; use the actual design-system source directory.
      {
        files: ["src/components/ui/**"],
        rules: {
          "shadcn/no-arbitrary-values": "off",
          "shadcn/no-restyle": "off",
          "shadcn/require-static-classes": "off",
        },
      },
    ],
    rules: {
      "eslint-js/no-restricted-syntax": [
        "error",
        {
          message: "Reserve export {} for multiple exports.",
          selector:
            "ExportNamedDeclaration[source=null][specifiers.length=1]",
        },
        {
          message: "Use React.* for React types instead of named type imports.",
          selector:
            "ImportDeclaration[source.value='react'][importKind='type'] > ImportSpecifier, ImportDeclaration[source.value='react'] > ImportSpecifier[importKind='type']",
        },
      ],
      "no-unused-vars": [
        "error",
        {
          fix: {
            imports: "safe-fix",
            variables: "off",
          },
        },
      ],
      // These five rules apply only with the Tailwind v4 @shadcn/lint plugin.
      "shadcn/no-arbitrary-values": [
        "error",
        {
          allow: ["layout"],
        },
      ],
      "shadcn/no-raw-colors": "error",
      "shadcn/no-restyle": [
        "error",
        {
          allow: ["layout"],
        },
      ],
      "shadcn/no-unknown-classes": "error",
      "shadcn/require-static-classes": "error",
      "sonarjs/cognitive-complexity": ["error", 13],
      "stylistic/jsx-newline": [
        "error",
        {
          allowMultilines: true,
          prevent: true,
        },
      ],
      "stylistic/padding-line-between-statements": [
        "error",
        {
          blankLine: "always",
          next: "*",
          prev: [
            "multiline-block-like",
            "multiline-const",
            "multiline-let",
            "multiline-expression",
          ],
        },
        {
          blankLine: "always",
          next: [
            "multiline-block-like",
            "multiline-const",
            "multiline-let",
            "multiline-expression",
          ],
          prev: "*",
        },
        {
          blankLine: "never",
          next: ["singleline-const", "singleline-let"],
          prev: ["singleline-const", "singleline-let"],
        },
        {
          blankLine: "always",
          next: { selector: "Program > *" },
          prev: { selector: "Program > *" },
        },
        {
          blankLine: "any",
          next: "import",
          prev: "import",
        },
      ],
      "vite-plus/prefer-vite-plus-imports": "error",
    },
    settings: jsPluginSettings,
  },
});
```

## Lint plugin dependencies

Install the plugins referenced by the selected Ultracite JS-plugins presets as direct dev dependencies in the package that owns `vite.config.ts`. Oxlint resolves their specifiers from the project, so extending the presets alone is not enough. For the base js-plugins preset used above:

```sh
vp add -D eslint-plugin-github eslint-plugin-sonarjs oxlint-plugin-react-doctor
```

The example's additional `stylistic` and `eslint-js` entries also require `@stylistic/eslint-plugin` and `oxlint-plugin-eslint` as direct dev dependencies. Keep `jsPluginSettings` on the root `lint.settings`, as shown above. Install any additional plugins required by the selected TanStack preset only when the project uses TanStack. Check the installed preset's plugin specifiers and supported versions, then run the project's lint command to verify they load. See [Ultracite's releases](https://github.com/haydenbleasel/ultracite/releases).

The restricted-imports pattern allows `./` and `../` imports. Imports starting with `../../` must use `@/*`, or the project's equivalent source alias. Keep both TanStack imports, their `lint.extends` entries, and the route filename override only in TanStack projects. The filename override specifically applies to TanStack Router's file-based routes.

## Tailwind design-system linting

For Tailwind v4 projects, add [shadcn's lint plugin](https://github.com/shadcn-ui/lint) to the package that owns the lint configuration:

```sh
vp add -D @shadcn/lint
```

Keep the `"@shadcn/lint"` entry in `lint.jsPlugins` in `vite.config.ts`, alongside the existing plugins. It works with custom Tailwind components and themes; shadcn/ui is not required. Omit the dependency and entry for projects without Tailwind v4.

Check that Node.js is at least 20.19 and Vite+'s bundled Oxlint is at least 1.80. Use the project's existing lint command, such as `vp lint` or `vp check`, to verify the plugin loads. Keep Vite+'s lint configuration in `vite.config.ts`; do not add a parallel `.oxlintrc.json` just for this plugin.

Registration alone enables no rules. The five rules above are Mike's preferred baseline for Tailwind v4 projects. Preserve other existing rule policies. Use the project's accepted design system to configure discovery and exceptions.

| Rule | Policy and scope |
| --- | --- |
| [no-restyle](https://github.com/shadcn-ui/lint/blob/main/docs/rules/no-restyle.md) | Error with `allow: ["layout"]`. Callers can set margin, width, positioning, transforms, and text alignment on recognized components. Padding, gap, color, typography, and shape changes need component variants or an accepted component contract. |
| [no-raw-colors](https://github.com/shadcn-ui/lint/blob/main/docs/rules/no-raw-colors.md) | Error. Use declared theme colors rather than raw palette classes or undeclared tokens. Also checks literal intrinsic SVG color attributes. Keep enabled inside design-system sources. `white`, `black`, `transparent`, `current`, and `inherit` pass. Arbitrary color values belong to no-arbitrary-values. |
| [no-arbitrary-values](https://github.com/shadcn-ui/lint/blob/main/docs/rules/no-arbitrary-values.md) | Error with `allow: ["layout"]`, following upstream's appearance-focused setup. `w-[320px]` passes; `p-[13px]`, `rounded-[10px]`, and `bg-[#333]` need tokens, scale values, or narrow exceptions. Arbitrary variants and CSS-variable shorthands are not arbitrary values. |
| [no-unknown-classes](https://github.com/shadcn-ui/lint/blob/main/docs/rules/no-unknown-classes.md) | Error. Checks whether the installed Tailwind v4 can generate a class using the project's theme, utilities, variants, and plugins. Keep enabled inside design-system sources. Add exact exceptions for external classes the app actually loads; an allowance does not generate CSS. |
| [require-static-classes](https://github.com/shadcn-ui/lint/blob/main/docs/rules/require-static-classes.md) | Error on recognized components and forwarding wrappers. Use complete literal classes, conditional choices, and readable class-helper arguments. Imported class values, unknown function results, and interpolated names such as `bg-${color}` cannot be checked. Plain elements are outside this rule. |

Disable `no-restyle`, `no-arbitrary-values`, and `require-static-classes` inside the design-system source directory so components can define internals and call their own variant functions. Keep `no-raw-colors` and `no-unknown-classes` enabled there. Adjust `src/components/ui/**` in the override above to the actual source directory. Omit all five rules and this override when the Tailwind v4 plugin does not apply.

Same-file constants can be read one hop deep. Wrappers may forward their received `className`; their authored defaults still get checked. Register custom class helpers only when their arguments describe their output. Do not assume that require-static-classes proves every class in the application is static.

Recognition and theme discovery are part of enforcement. Use `components.json` when present; verify the component aliases and theme CSS path it names. For a custom design system, set `settings.shadcn.ui` or `componentImports` for the actual component imports. Merge these settings without losing `jsPluginSettings`. Without `components.json`, verify the discovered stylesheet imports Tailwind and includes the project's tokens through its import graph. Scope component recognition per app in a workspace and preserve each app's theme discovery. Fix theme-loading warnings before treating a clean result as full coverage; no-unknown-classes falls back to a less precise grammar when Tailwind or the theme cannot load. See [settings](https://github.com/shadcn-ui/lint#settings) and [discovery and analysis limits](https://github.com/shadcn-ui/lint/blob/main/docs/how-it-works.md).

Exceptions apply to each rule independently. Allowing a color or arbitrary value does not make it acceptable to no-restyle. Component `contracts` match resolved names; a contract inherits omitted top-level keys, and only the last matching contract applies. Require-static-classes has no `allow`, `deny`, or `contracts`. Avoid `deny` alone unless the intention is to allow every other match. Keep exceptions narrow and supported by styles the app actually loads. Review suggested color replacements against the design; proximity in light-mode color values does not establish the right semantic token.

Add other `shadcn/*` rules when relevant. See the upstream [setup guide](https://github.com/shadcn-ui/lint/blob/main/SETUP.md), [rules](https://github.com/shadcn-ui/lint/blob/main/docs/rules.md), and [design-system configuration](https://github.com/shadcn-ui/lint/blob/main/docs/design-systems.md).

## Framework plugins and editor settings

For a React, TanStack Router, and Tailwind app, add the current project plugins for TanStack Router with automatic code splitting, React with the Oxc React compiler integration, and Tailwind. Enable TypeScript path resolution. Confirm the exact plugin APIs against the installed versions.

When setting up Tailwind's Vite integration, Mike prefers `@tailwindcss/vite` as a dev dependency in the app package:

```sh
vp add -D @tailwindcss/vite
```

### Oxc React compiler

When using the Oxc React compiler integration, install `oxc-transform-react` as a dev dependency in the app package. Remove Babel imports, plugin options, configuration files, and direct dependencies used solely by the replaced React compiler setup. Do not leave that Babel compiler active alongside Oxc. Preserve Babel configuration needed by another build path; StyleX's unplugin may also require Babel internally. Verify the installed React plugin's Oxc compiler option and run the build. See the [Oxc React compiler guide](https://oxc.rs/docs/guide/usage/transformer/react-compiler).

### StyleX

When using StyleX, use `@stylexjs/stylex` as the runtime package and `@stylexjs/unplugin` as the Vite-plugin dev dependency. Create `src/lib/sx.ts`, or use the project's equivalent shared-library path:

```ts
import type { StyleXStyles } from "@stylexjs/stylex";

// oxlint-disable-next-line sonarjs/no-wildcard-import, sx intentionally exports the stylex namespace
export * as sx from "@stylexjs/stylex";

export type WithStyleX<T> = T & {
  stylex?: StyleXStyles;
};
```

Use `import { sx } from "@/lib/sx"` and `WithStyleX<Props>` when a component accepts an optional StyleX override. This namespace helper is an intentional exception to the general preference against barrels.

Configure the StyleX Vite plugin's `importSources` to match the helper's exact import specifier:

```ts
import stylex from "@stylexjs/unplugin/vite";

// Merge into the existing Vite plugins array.
stylex({
  importSources: [{ as: "sx", from: "@/lib/sx" }],
});
```

Replace `@/lib/sx` with the project's actual helper path in both the imports and `importSources`. Preserve entries needed by any existing direct StyleX imports.

When the installed plugin declares its Vite export as `any`, add a local `stylex-unplugin.d.ts` included by the tsconfig that checks `vite.config.ts`:

```ts
declare module "@stylexjs/unplugin/vite" {
  import type { UserOptions } from "@stylexjs/unplugin";
  import type { Plugin } from "vite-plus";

  const stylex: (options?: Partial<UserOptions>) => Plugin;

  export default stylex;
}
```

Keep this declaration file free of top-level imports or exports so it declares the module rather than trying to augment an untyped export. Check the installed package's options type and Vite compatibility, then verify both type checking and a build that compiles an `sx.create` call imported through the helper. See the [StyleX unplugin](https://stylexjs.com/docs/api/configuration/unplugin/) and [importSources configuration](https://stylexjs.com/docs/api/configuration/babel-plugin/#importsources).

### TanStack Router editor settings

When TanStack Router is present, merge these into the project's `.vscode/settings.json` without replacing unrelated settings:

```json
{
  "files.readonlyInclude": {
    "**/routeTree.gen.ts": true
  },
  "files.watcherExclude": {
    "**/routeTree.gen.ts": true
  },
  "search.exclude": {
    "**/routeTree.gen.ts": true
  }
}
```
