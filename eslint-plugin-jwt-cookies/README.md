# eslint-plugin-jwt-cookies

[![npm](https://img.shields.io/npm/v/@boring-stack-pkg/eslint-plugin-jwt-cookies?logo=npm)](https://www.npmjs.com/package/@boring-stack-pkg/eslint-plugin-jwt-cookies) [![source](https://img.shields.io/badge/source-github-blue?logo=github)](https://github.com/boringstack-xyz/eslint-plugins/tree/main/eslint-plugin-jwt-cookies)

ESLint rules that harden auth-cookie + password-hashing defaults:

- **`auth-cookie-must-be-httponly`** — auth cookies must set
  `httpOnly: true` (or spread a trusted cookie-config helper).
  JS-readable session cookies leak via XSS.
- **`auth-cookie-must-be-secure-in-prod`** — auth cookies must set
  `secure:` to `true` or an env-derived expression. Cookies leak over
  plain HTTP without it.
- **`bcrypt-rounds-min`** — `bcrypt.hash` / `hashSync` must use a rounds
  value at least `minRounds` (default 10).

This plugin is _defense in depth_. The cookie-config helper pattern
(`AUTH_COOKIE_CONFIG`) is the primary safeguard — these rules enforce
that the helper is actually used, and that ad-hoc cookie writes don't
quietly bypass it.

## Install

```sh
pnpm add -D @boring-stack-pkg/eslint-plugin-jwt-cookies
```

Peer deps: `eslint >= 8.57`, `@typescript-eslint/parser >= 8`,
`typescript >= 5`.

## Use (flat config)

```js
import tsParser from "@typescript-eslint/parser";
import jwtCookies from "@boring-stack-pkg/eslint-plugin-jwt-cookies";

export default [
  {
    files: ["**/*.{ts,tsx}"],
    languageOptions: { parser: tsParser },
    plugins: { "jwt-cookies": jwtCookies },
    rules: {
      "jwt-cookies/auth-cookie-must-be-httponly": "error",
      "jwt-cookies/auth-cookie-must-be-secure-in-prod": "error",
      "jwt-cookies/bcrypt-rounds-min": ["error", { minRounds: 12 }],
    },
  },
];
```

Or use the bundled config:

```js
import jwtCookies from "@boring-stack-pkg/eslint-plugin-jwt-cookies";

export default [jwtCookies.configs.recommended];
```

## Rules

| Rule                                                                                     | Description                                                        | Fixable |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | ------- |
| [`auth-cookie-must-be-httponly`](docs/rules/auth-cookie-must-be-httponly.md)             | Auth cookies must set `httpOnly: true`                             | –       |
| [`auth-cookie-must-be-secure-in-prod`](docs/rules/auth-cookie-must-be-secure-in-prod.md) | Auth cookies must set `secure:` (literal `true` or env expression) | –       |
| [`bcrypt-rounds-min`](docs/rules/bcrypt-rounds-min.md)                                   | `bcrypt.hash` rounds must meet a minimum                           | –       |

## License

MIT.
