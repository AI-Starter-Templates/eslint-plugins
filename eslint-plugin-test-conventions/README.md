# eslint-plugin-test-conventions

[![npm](https://img.shields.io/npm/v/@boring-stack-pkg/eslint-plugin-test-conventions?logo=npm)](https://www.npmjs.com/package/@boring-stack-pkg/eslint-plugin-test-conventions) [![source](https://img.shields.io/badge/source-github-blue?logo=github)](https://github.com/boringstack-xyz/eslint-plugins/tree/main/eslint-plugin-test-conventions)

ESLint rules that catch the most common test-suite mistakes:

- **`no-focused-tests`** — `test.only` / `it.only` / `fdescribe` / `fit`
  left in committed code. The single `.only` that turns CI green by
  skipping everything else.
- **`no-direct-db-in-tests`** — tests reaching into the DB driver
  directly, bypassing the helpers entrypoint where you do connection
  probing, isolation, and cleanup.
- **`test-file-mirrors-source`** — orphaned `*.test.ts` files left
  behind after a refactor, still passing, still giving you false
  confidence in code that no longer exists.

## Install

```sh
pnpm add -D @boring-stack-pkg/eslint-plugin-test-conventions
```

Peer deps: `eslint >= 8.57`, `@typescript-eslint/parser >= 8`,
`typescript >= 5`.

## Use (flat config)

```js
import tsParser from "@typescript-eslint/parser";
import testConventions from "@boring-stack-pkg/eslint-plugin-test-conventions";

export default [
  {
    files: ["**/*.{ts,tsx}"],
    languageOptions: { parser: tsParser },
    plugins: { "test-conventions": testConventions },
    rules: {
      "test-conventions/no-focused-tests": "error",
      "test-conventions/no-direct-db-in-tests": "error",
      "test-conventions/test-file-mirrors-source": "error",
    },
  },
];
```

Or use the bundled config:

```js
import testConventions from "@boring-stack-pkg/eslint-plugin-test-conventions";

export default [testConventions.configs.recommended];
```

## Rules

| Rule                                                                 | Description                                            | Fixable |
| -------------------------------------------------------------------- | ------------------------------------------------------ | ------- |
| [`no-focused-tests`](docs/rules/no-focused-tests.md)                 | Disallow `.only` and bare aliases (`fdescribe`, `fit`) | –       |
| [`no-direct-db-in-tests`](docs/rules/no-direct-db-in-tests.md)       | Tests must go through the helpers entrypoint           | –       |
| [`test-file-mirrors-source`](docs/rules/test-file-mirrors-source.md) | Every test file must mirror an existing source file    | –       |

## License

MIT.
