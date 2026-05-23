# eslint-plugin-cache-keys

[![npm](https://img.shields.io/npm/v/@boring-stack-pkg/eslint-plugin-cache-keys?logo=npm)](https://www.npmjs.com/package/@boring-stack-pkg/eslint-plugin-cache-keys) [![source](https://img.shields.io/badge/source-github-blue?logo=github)](https://github.com/boringstack-xyz/eslint-plugins/tree/main/eslint-plugin-cache-keys)

ESLint rules preventing the most common cache-layer bugs:

- **`cache-set-must-have-ttl`** — cache writes without a TTL field
  accumulate forever and OOM Redis.
- **`cache-key-must-be-prefixed`** — cache keys must start with a
  configured namespace prefix to prevent collisions when multiple apps
  share a Redis instance.
- **`cache-key-from-helper`** _(opt-in)_ — cache keys must come from a
  configured helper function. Forces a single source of truth and makes
  invalidation tractable across a codebase.

The recommended config enables rules 1 and 2 at `error`. Rule 3 is opt-in
because it requires you to declare your project's key-builder helpers.

## Install

```sh
pnpm add -D @boring-stack-pkg/eslint-plugin-cache-keys
```

Peer deps: `eslint >= 8.57`, `@typescript-eslint/parser >= 8`,
`typescript >= 5`.

## Use (flat config)

```js
import tsParser from "@typescript-eslint/parser";
import cacheKeys from "@boring-stack-pkg/eslint-plugin-cache-keys";

export default [
  {
    files: ["**/*.{ts,tsx}"],
    languageOptions: { parser: tsParser },
    plugins: { "cache-keys": cacheKeys },
    rules: {
      "cache-keys/cache-set-must-have-ttl": "error",
      "cache-keys/cache-key-must-be-prefixed": "error",
      "cache-keys/cache-key-from-helper": [
        "error",
        { helperNames: ["userCacheKey", "stripeEventCacheKey"] },
      ],
    },
  },
];
```

Or use the bundled config (rules 1–2 enabled, rule 3 off):

```js
import cacheKeys from "@boring-stack-pkg/eslint-plugin-cache-keys";

export default [cacheKeys.configs.recommended];
```

## Rules

| Rule                                                                     | Description                              | Default in recommended |
| ------------------------------------------------------------------------ | ---------------------------------------- | ---------------------- |
| [`cache-set-must-have-ttl`](docs/rules/cache-set-must-have-ttl.md)       | `.set` calls must include `ttlSeconds`   | `error`                |
| [`cache-key-must-be-prefixed`](docs/rules/cache-key-must-be-prefixed.md) | Keys must start with a configured prefix | `error`                |
| [`cache-key-from-helper`](docs/rules/cache-key-from-helper.md)           | Keys must be built by configured helpers | `off` (opt-in)         |

## License

MIT.
