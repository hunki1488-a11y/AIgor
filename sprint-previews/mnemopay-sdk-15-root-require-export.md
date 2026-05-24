# MnemoPay SDK #15 Root Require Export Sprint Preview

Target issue: https://github.com/mnemopay/mnemopay-sdk/issues/15

Public patch: https://github.com/hunki1488-a11y/AIgor/blob/main/submissions/mnemopay-sdk-15-root-require-export.patch

## Scope

This preview fixes the root CommonJS consumer path for `require("@mnemopay/sdk")`.

The current root export map has `import` and `types`, but no `require` condition. With an `exports` map present, Node resolves through that map before falling back to `main`, so CommonJS consumers can fail even though `main` points at `dist/index.js`.

Included:

- add root `exports["."].require = "./dist/index.js"`
- add a regression test that builds a temporary consumer `node_modules/@mnemopay/sdk`
- verify `require("@mnemopay/sdk")` resolves through the real package export map
- keep existing root-import side-effect protection test unchanged

## Verification

Commands run locally against a fresh clone:

```bash
npm install --ignore-scripts
npm run build
npx vitest run tests/root-require-export.test.ts tests/no-side-effect-on-root-import.test.ts
git diff --check
```

Result:

```text
npm run build passed
2 test files passed, 3 tests passed
git diff --check passed
```

## Paid Sprint Shape

Low-risk paid scope:

- 250 USD: open a focused PR with this patch and acceptance notes.
- 500 USD: include maintainer review iteration plus a small export-map audit for the documented subpath imports.

Turnaround target: 24-48 hours after scope confirmation.
