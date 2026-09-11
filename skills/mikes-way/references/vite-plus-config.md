# vite+ configuration

this is the linting and formatting portion of mike's current `vite.config.ts`. keep it when the installed versions support it and the repo has not chosen different rules. remove framework-specific presets or overrides when they do not apply.

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
