---
type: Web Page
title: TypeScript - Bun
description: Using TypeScript with Bun, including type definitions and compiler options
resource: https://bun.sh/docs/typescript
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`@types/bun`.
terminal

`Bun` global in your TypeScript files without errors in your editor.
## Suggested `compilerOptions`

Bun supports top-level await, JSX, and imports with `.ts` extensions, which TypeScript doesn’t allow by default. Use these `compilerOptions` in a Bun project so TypeScript doesn’t warn about those features.
tsconfig.json

`bun init` in a new directory generates this `tsconfig.json` for you.
terminal

## TypeScript 6 and 7

If you’re using TypeScript 6.0 or later, you also need`"types": ["bun"]` in your `compilerOptions`. See TypeScript 6 and 7.

# Citations

1. Source page: https://bun.sh/docs/typescript
