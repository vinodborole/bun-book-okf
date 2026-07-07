---
type: Web Page
title: TypeScript 6 and 7 - Bun
description: How to configure Bun's type definitions for TypeScript 6.0 and 7.0, which
  no longer auto-discover @types packages. Fix 'Cannot find name Bun' and other missing
  type errors after upgrading TypeScript.
resource: https://bun.sh/docs/typescript-6
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`Bun`, `Request`, or other globals from `@types/bun`, here’s how to fix it.
## What changed

Starting in TypeScript 6.0, the`types` field in `compilerOptions` defaults to an empty array instead of including all `@types/*` packages. You now need to explicitly list the type packages you use.
## Add `"types": ["bun"]` to your tsconfig

In your `tsconfig.json`, add `"types": ["bun"]` to `compilerOptions`:
tsconfig.json

`types` array tells TypeScript to load type definitions from `@types/bun`. If you use other `@types/*` packages, include them too:
tsconfig.json

`@types/bun` installed — the `types` option tells TypeScript *which*packages to include, but the package itself must exist in

`node_modules`:
terminal

## Full recommended tsconfig.json

Here’s the full recommended`tsconfig.json` for a Bun project using TypeScript 6.0 or later:
tsconfig.json

## Does this apply to TypeScript 7?

Yes. TypeScript 7 carries forward the same default. If you’re upgrading directly from TypeScript 5 to 7, the same fix applies — add`"types": ["bun"]` to your `compilerOptions`.

# Citations

1. Source page: https://bun.sh/docs/typescript-6
