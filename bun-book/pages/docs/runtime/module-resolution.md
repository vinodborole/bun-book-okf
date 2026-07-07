---
type: Web Page
title: Module Resolution - Bun
description: How Bun resolves modules and handles imports in JavaScript and TypeScript
resource: https://bun.sh/docs/runtime/module-resolution
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

## Syntax

Consider the following files.`index.ts` prints “Hello world!”.
terminal

`./hello` is a relative path with no extension. **Extensioned imports are optional but supported.**To resolve this import, Bun checks for the following files in order:

- `./hello.tsx`
- `./hello.jsx`
- `./hello.ts`
- `./hello.mjs`
- `./hello.js`
- `./hello.cjs`
- `./hello.json`
- `./hello/index.tsx`
- `./hello/index.jsx`
- `./hello/index.ts`
- `./hello/index.mjs`
- `./hello/index.js`
- `./hello/index.cjs`
- `./hello/index.json`

index.ts

`from "*.js{x}"`, Bun also checks for a matching `*.ts{x}` file, to be compatible with TypeScript’s ES module support.
index.ts

`import`/`export` syntax) and CommonJS modules (`require()`/`module.exports`). The following CommonJS version also works in Bun.
## Module systems

Bun has native support for CommonJS and ES modules. ES modules are the recommended module format for new projects, but CommonJS modules are still widely used in the Node.js ecosystem. In Bun’s JavaScript runtime, both ES modules and CommonJS modules can use`require`. If the target module is an ES module, `require` returns the module namespace object (equivalent to `import * as`). If the target module is a CommonJS module, `require` returns the `module.exports` object (as in Node.js).
| Module Type | `require()` | `import * as` | 
|---|---|---|
| ES Module | Module Namespace | Module Namespace | 
| CommonJS | module.exports | `default`is`module.exports`, keys of module.exports are named exports | 

### Using `require()`

You can `require()` any file or package, even `.ts` or `.mjs` files.
index.ts

What is a CommonJS module?

What is a CommonJS module?

In 2016, ECMAScript added support for ES modules. ES modules are the standard for JavaScript modules. However, millions of npm packages still use CommonJS modules.CommonJS modules use The biggest difference between CommonJS and ES modules is that CommonJS modules are synchronous, while ES modules are asynchronous. Other differences:

`module.exports` to export values and are typically imported with `require`.my-commonjs.cjs

- ES modules support top-level `await`and CommonJS modules don’t.
- ES modules are always in strict mode, while CommonJS modules are not.
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
Fortunately, very few libraries use top-level await, so this is rarely a problem. But if you’re using top-level await in your application code, make sure that file isn’t `require()`’d from elsewhere in your application. Use `import` or dynamic `import()` instead.
## Importing packages

Bun implements the Node.js module resolution algorithm, so you can import packages from`node_modules` with a bare specifier.
index.ts

`from "foo"`, Bun scans up the file system for a `node_modules` directory containing the package `foo`.
### NODE_PATH

Bun supports`NODE_PATH` for additional module resolution directories:
`:` on Unix, `;` on Windows):
`foo` package, Bun reads its `package.json` to determine the package’s entrypoint. Bun first reads the `exports` field and checks for the following conditions.
package.json

*first*in the

`package.json` determines the package’s entrypoint.
Bun respects subpath `"exports"` and `"imports"`.
package.json

package.json

`"exports"` map prevents other subpaths from being importable; you can only import files that are explicitly exported. Given the preceding `package.json`:
index.ts

**Shipping TypeScript**— Bun supports the special

`"bun"` export condition. If your library is written in TypeScript,
you can publish your un-transpiled TypeScript files to `npm` directly. If you specify your package’s `*.ts` entrypoint
in the `"bun"` condition, Bun imports and executes your TypeScript source files directly.`exports` is not defined, Bun falls back to `"module"` (ESM imports only) then `"main"`.
package.json

### Custom conditions

The`--conditions` flag specifies the conditions to use when resolving packages from `package.json` `"exports"`.
Both `bun build` and Bun’s runtime support this flag.
terminal

`conditions` programmatically with `Bun.build`:
build.ts

## Path re-mapping

Bun supports import path re-mapping through TypeScript’s`compilerOptions.paths` in `tsconfig.json`, which works well with editors. If you aren’t a TypeScript user, use a `jsconfig.json` in your project root for the same behavior.
tsconfig.json

`package.json`, where mapped paths must start with `#`. This approach doesn’t work as well with editors, but you can use both options together.
package.json

Low-level details of CommonJS interop in Bun

Low-level details of CommonJS interop in Bun

Bun’s JavaScript runtime has native support for CommonJS. When Bun’s JavaScript transpiler detects usages of 

`module.exports`, it treats the file as CommonJS. The module loader then wraps the transpiled module in a function shaped like this:`module`, `exports`, and `require` are very much like the `module`, `exports`, and `require` in Node.js. These are assigned through a `with scope` in C++. An internal `Map` stores the `exports` object to handle cyclical `require` calls before the module is fully loaded.Once the CommonJS module is successfully evaluated, a Synthetic Module Record is created with the `default` ES Module export set to `module.exports` and keys of the `module.exports` object are re-exported as named exports (if the `module.exports` object is an object).Bun’s bundler works differently: it wraps the CommonJS module in a `require_${moduleName}` function which returns the `module.exports` object.`import.meta`

The `import.meta` object exposes information about the current module. It’s part of the JavaScript language, but its contents are not standardized: each “host” (browser or runtime) implements its own properties on the `import.meta` object.
Bun implements the following properties.
/path/to/project/file.ts

| Property | Description | 
|---|---|
| `import.meta.dir` | Absolute path to the directory containing the current file, e.g. `/path/to/project`. Equivalent to`__dirname`in CommonJS modules (and Node.js) | 
| `import.meta.dirname` | An alias to `import.meta.dir`, for Node.js compatibility | 
| `import.meta.env` | An alias to `process.env`. | 
| `import.meta.file` | The name of the current file, e.g. `index.tsx` | 
| `import.meta.path` | Absolute path to the current file, e.g. `/path/to/project/index.ts`. Equivalent to`__filename`in CommonJS modules (and Node.js) | 
| `import.meta.filename` | An alias to `import.meta.path`, for Node.js compatibility | 
| `import.meta.main` | Indicates whether the current file is the entrypoint to the current `bun`process:`true`if it’s executed directly by`bun run`,`false`if it’s imported | 
| `import.meta.resolve` | Resolve a module specifier (e.g. `"zod"`or`"./file.tsx"`) to a url. Equivalent to`import.meta.resolve`in browsers. Example:`import.meta.resolve("zod")`returns`"file:///path/to/project/node_modules/zod/index.ts"` | 
| `import.meta.url` | A `string`url to the current file, e.g.`file:///path/to/project/index.ts`. Equivalent to`import.meta.url`in browsers |

# Citations

1. Source page: https://bun.sh/docs/runtime/module-resolution
