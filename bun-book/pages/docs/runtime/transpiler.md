---
type: Web Page
title: Transpiler - Bun
description: Use Bun's transpiler to transpile JavaScript and TypeScript code
resource: https://bun.sh/docs/runtime/transpiler
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`Bun.Transpiler` class. To create an instance:
`.transformSync()`

Transpile code synchronously with the `.transformSync()` method. Modules are not resolved and the code is not executed. The result is a string of vanilla JavaScript code.
`new Bun.Transpiler()` constructor, pass a second argument to `.transformSync()`.
Nitty gritty

Nitty gritty

`.transformSync` runs the transpiler in the same thread as the calling code.Macros run in the same thread as the transpiler, but in a separate event loop from the rest of your application. Macros and regular code share globals, so it is possible (but not recommended) to share state between them. Using AST nodes outside of a macro is undefined behavior.`.transform()`

The `transform()` method is an async version of `.transformSync()` that returns a `Promise<string>`.
*many*large files, use

`Bun.Transpiler.transformSync`. The threadpool overhead often costs more than the transpilation itself.
Nitty gritty

Nitty gritty

The 

`.transform()` method runs the transpiler in Bun’s worker threadpool, so running it 100 times spreads the work across `Math.floor($cpu_count * 0.8)` threads without blocking the main JavaScript thread.If your code uses a macro, it may spawn a new copy of Bun’s JavaScript runtime environment in that new thread.`.scan()`

The `.scan()` method scans source code and returns a list of its imports and exports, plus metadata about each one. Type-only imports and exports are ignored.
`imports` array has a `path` and `kind`. Bun categorizes imports into the following kinds:
- `import-statement`:- `import React from 'react'`
- `require-call`:- `const val = require('./cjs.js')`
- `require-resolve`:- `require.resolve('./cjs.js')`
- `dynamic-import`:- `import('./loader')`
- `import-rule`:- `@import 'foo.css'`
- `url-token`:- `url('./foo.png')`

`.scanImports()`

In performance-sensitive code, use the `.scanImports()` method to get a list of imports. It’s faster than `.scan()` (especially for large files) but marginally less accurate due to its performance optimizations.
## Reference

See Typescript Definitions

# Citations

1. Source page: https://bun.sh/docs/runtime/transpiler
