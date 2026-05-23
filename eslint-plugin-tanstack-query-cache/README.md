# eslint-plugin-tanstack-query-cache

[![npm](https://img.shields.io/npm/v/@boring-stack-pkg/eslint-plugin-tanstack-query-cache?logo=npm)](https://www.npmjs.com/package/@boring-stack-pkg/eslint-plugin-tanstack-query-cache) [![source](https://img.shields.io/badge/source-github-blue?logo=github)](https://github.com/boringstack-xyz/eslint-plugins/tree/main/eslint-plugin-tanstack-query-cache)

ESLint rules that catch TanStack Query cache bugs when list keys are built with spreads (`queryKey: [...LIST_KEY, filter]`) but mutations still call `setQueryData(LIST_KEY, …)` — which only updates one cache entry.

## Rules

| Rule                                                                                                       | Description                                                                                                                  |
| ---------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| [`prefix-query-key-must-use-set-queries-data`](./docs/rules/prefix-query-key-must-use-set-queries-data.md) | Prefer `setQueriesData({ queryKey: prefix }, …)` and matcher-style `cancelQueries` when hooks extend the key after a spread. |

## Usage (flat config)

```js
import tanstackQueryCache from "@boring-stack-pkg/eslint-plugin-tanstack-query-cache";

export default [
  {
    files: ["**/*.queries.ts"],
    plugins: { "tanstack-query-cache": tanstackQueryCache },
    rules: {
      "tanstack-query-cache/prefix-query-key-must-use-set-queries-data":
        "error",
    },
  },
];
```

## Development

```bash
pnpm install
pnpm test
pnpm build
```
