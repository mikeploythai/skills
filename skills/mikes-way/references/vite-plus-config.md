# vite+ configuration

this is mike's starting linting and formatting configuration for `vite.config.ts`. keep it when the installed versions support it and the repo has not chosen different rules. remove framework-specific presets, js plugins, and overrides when they do not apply. include `@shadcn/lint` only for compatible tailwind v4 projects, as described below.

```ts
import ultracite from "ultracite/oxfmt";
import antiSlop from "ultracite/oxlint/anti-slop";
import core from "ultracite/oxlint/core";
import jsPlugins, { jsPluginSettings } from "ultracite/oxlint/js-plugins";
import reactLint from "ultracite/oxlint/react";
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
      tanstack,
      vitest,
      jsPlugins,
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
                  message: "Use the @/* alias for imports between src modules.",
                  regex: "^\\.\\./",
                },
              ],
            },
          ],
        },
      },
      {
        files: ["src/routes/**/*.tsx"],
        rules: {
          "github/filenames-match-regex": "off",
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

## tailwind design-system linting

for tailwind v4 projects, add [shadcn's lint plugin](https://github.com/shadcn-ui/lint)
to the package that owns the lint configuration:

```sh
vp add -D @shadcn/lint
```

keep the `"@shadcn/lint"` entry in `lint.jsPlugins` in `vite.config.ts`, alongside
the existing plugins. it works with custom tailwind components and themes;
shadcn/ui is not required. omit the dependency and entry for projects without
tailwind v4.

check that node.js is at least 20.19 and vite+'s bundled oxlint is at least 1.80.
use the project's existing lint command, such as `vp lint` or `vp check`, to
verify the plugin loads. keep vite+'s lint configuration in `vite.config.ts`;
do not add a parallel `.oxlintrc.json` just for this plugin.

registration alone enables no rules. preserve existing rule policies, then
configure the `shadcn/*` rules, discovery settings, and component contracts
that match the project's accepted design system. see the upstream
[setup guide](https://github.com/shadcn-ui/lint/blob/main/SETUP.md),
[rules](https://github.com/shadcn-ui/lint/blob/main/docs/rules.md), and
[design-system configuration](https://github.com/shadcn-ui/lint/blob/main/docs/design-systems.md).

## framework plugins and editor settings

for a react, tanstack router, and tailwind app, add the current project plugins for tanstack router with automatic code splitting, react with the oxc react compiler integration, and tailwind. enable typescript path resolution. confirm the exact plugin apis against the installed versions.

when tanstack router is present, merge these into the project's
`.vscode/settings.json` without replacing unrelated settings:

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
