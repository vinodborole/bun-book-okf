---
type: Web Page
title: Module Resolution - Bun
description: How Bun resolves modules and handles imports in JavaScript and TypeScript
resource: https://bun.sh/docs/runtime/module-resolution
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

## Syntax

Consider the following files.`index.ts` prints “Hello world!”.
terminal

`./hello` is a relative path with no extension. **Extensioned imports are optional but supported.**To resolve this import, Bun checks for the following files in order:

- `./hello.tsx`
- `./hello.jsx`
- `./hello.mts`
- `./hello.ts`
- `./hello.mjs`
- `./hello.js`
- `./hello.cts`
- `./hello.cjs`
- `./hello.json`
- `./hello/index.tsx`
- `./hello/index.jsx`
- `./hello/index.mts`
- `./hello/index.ts`
- `./hello/index.mjs`
- `./hello/index.js`
- `./hello/index.cts`
- `./hello/index.cjs`
- `./hello/index.json`

The exact order varies by context: 

`require()` tries CommonJS extensions (`.cts`, `.cjs`) before ESM ones (`.mts`,
`.mjs`), and imports inside `node_modules` try JavaScript extensions before TypeScript ones. The list above shows the
order for a local ESM `import`.`./hello.world` can resolve to `./hello.world.ts`).
index.ts

`from "*.js"` or `from "*.jsx"`, Bun also checks for a matching `*.ts` or `*.tsx` file, and outside `node_modules` `from "*.mjs"` also matches `*.mts`. This follows the TypeScript compiler’s [file extension substitution](https://www.typescriptlang.org/docs/handbook/modules/reference.html#file-extension-substitution), which lets source files reference each other by their compiled output paths. Note that unlike TypeScript, Bun doesn’t rewrite

`.cjs` to `.cts`.
index.ts

`import`/`export` syntax) and CommonJS modules (`require()`/`module.exports`). The following CommonJS version also works in Bun.
## Module systems

Bun has native support for CommonJS and ES modules. ES modules are the recommended module format for new projects, but CommonJS modules are still widely used in the Node.js ecosystem. In Bun’s JavaScript runtime, both ES modules and CommonJS modules can use`require`. If the target module is an ES module, `require` returns the module namespace object (equivalent to `import * as`). If the target module is a CommonJS module, `require` returns the `module.exports` object (as in Node.js).
### Using `require()`

You can `require()` any file or package, even `.ts` or `.mjs` files.
index.ts

What is a CommonJS module?

What is a CommonJS module?

In 2016, ECMAScript added support for The biggest difference between CommonJS and ES modules is that CommonJS modules are synchronous, while ES modules are asynchronous. Other differences:

[ES modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules). ES modules are the standard for JavaScript modules. However, millions of npm packages still use CommonJS modules.CommonJS modules use`module.exports` to export values and are typically imported with `require`.my-commonjs.cjs

- ES modules support top-level `await`and CommonJS modules don’t.
- ES modules are always in [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode), while CommonJS modules are not.
- Browsers do not have native support for CommonJS modules, but they do have native support for ES modules through `<script type="module">`.
- CommonJS modules are not statically analyzable, while ES modules only allow static imports and exports.
- Static `import`statements run synchronously, just like CommonJS`require`. ES modules can also be loaded on the fly with the asynchronous`import()`function, called a “dynamic import”.

### Using `import`

You can `import` any file or package, even `.cjs` files.
index.ts

### Using `import` and `require()` together

In Bun, you can use `import` or `require` in the same file—they both work, all the time.
index.ts

### Top level await

The only exception to this rule is top-level await. You can’t`require()` a file that uses top-level await, since the `require()` function is inherently synchronous.
Fortunately, very few libraries use top-level await, so this is rarely a problem. But if you’re using top-level await in your application code, make sure that file isn’t `require()`’d from elsewhere in your application. Use `import` or [dynamic](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import)instead.

`import()`## Importing packages

Bun implements the Node.js module resolution algorithm, so you can import packages from`node_modules` with a bare specifier.
index.ts

[Node.js documentation](https://nodejs.org/api/modules.html). Briefly: if you import

`from "foo"`, Bun scans up the file system for a `node_modules` directory containing the package `foo`.
### NODE_PATH

Bun supports`NODE_PATH` for additional module resolution directories:
`:` on Unix, `;` on Windows):
`foo` package, Bun reads its `package.json` to determine the package’s entrypoint. Bun first reads the `exports` field and checks for the following conditions.
package.json

*first*in the

`package.json` determines the package’s entrypoint.
Bun respects subpath [and](https://nodejs.org/api/packages.html#subpath-exports)

`"exports"`[.](https://nodejs.org/api/packages.html#imports)

`"imports"`package.json

package.json

`"exports"` map prevents other subpaths from being importable; you can only import files that are explicitly exported. Given the preceding `package.json`:
index.ts

**Shipping TypeScript**— Bun supports the special

`"bun"` export condition. If your library is written in TypeScript,
you can publish your un-transpiled TypeScript files to `npm` directly. If you specify your package’s `*.ts` entrypoint
in the `"bun"` condition, Bun imports and executes your TypeScript source files directly.`exports` is not defined, Bun falls back to the legacy top-level entrypoint fields. At runtime Bun prefers [(or an implicit](https://nodejs.org/api/packages.html#main)

`"main"``index.*` file) when present, and uses `"module"` otherwise.
package.json

### Custom conditions

The`--conditions` flag specifies the conditions to use when resolving packages from `package.json` `"exports"`.
Both `bun build` and Bun’s runtime support this flag.
terminal

`conditions` programmatically with `Bun.build`:
build.ts

## Path re-mapping

Bun supports import path re-mapping through TypeScript’s[in](https://www.typescriptlang.org/tsconfig#paths)

`compilerOptions.paths``tsconfig.json`, which works well with editors. If you aren’t a TypeScript user, use a [in your project root for the same behavior.](https://code.visualstudio.com/docs/languages/jsconfig)

`jsconfig.json`tsconfig.json

[Node.js-style subpath imports](https://nodejs.org/api/packages.html#subpath-imports)in

`package.json`, where mapped paths must start with `#`. TypeScript and editors resolve these too, and you can use both mechanisms together.
package.json

Low-level details of CommonJS interop in Bun

Low-level details of CommonJS interop in Bun

Bun’s JavaScript runtime has native support for CommonJS. When Bun’s JavaScript transpiler detects usages of 

`module.exports`, it treats the file as CommonJS. The module loader then wraps the transpiled module in a function shaped like this:`module`, `exports`, and `require` are very much like the `module`, `exports`, and `require` in Node.js. These are assigned through a [in C++. An internal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/with)`with scope``Map` stores the `exports` object to handle cyclical `require` calls before the module is fully loaded.Once the CommonJS module is successfully evaluated, a Synthetic Module Record is created with the `default` ES Module [export set to](https://github.com/oven-sh/bun/blob/9b6913e1a674ceb7f670f917fc355bb8758c6c72/src/bun.js/bindings/CommonJSModuleRecord.cpp#L212-L213)and keys of the`module.exports``module.exports` object are re-exported as named exports (if the `module.exports` object is an object).Bun’s bundler works differently: it wraps the CommonJS module in a `require_${moduleName}` function which returns the `module.exports` object.`import.meta`

The `import.meta` object exposes information about the current module. It’s part of the JavaScript language, but its contents are not standardized: each “host” (browser or runtime) implements its own properties on the `import.meta` object.
Bun implements the following properties.
/path/to/project/file.ts

# Citations

1. Source page: https://bun.sh/docs/runtime/module-resolution
