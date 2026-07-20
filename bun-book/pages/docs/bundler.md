---
type: Web Page
title: Bundler - Bun
description: Bun's fast native bundler for JavaScript, TypeScript, JSX, and more
resource: https://bun.sh/docs/bundler
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

`bun build` CLI command or the `Bun.build()` JavaScript API.
### At a Glance

- JS API: `await Bun.build({ entrypoints, outdir })`
- CLI: `bun build <entry> --outdir ./out`
- Watch: `--watch`for incremental rebuilds
- Targets: `--target browser|bun|node`
- Formats: `--format esm|cjs|iife`(experimental for cjs/iife)

- JavaScript
- CLI

build.ts

[three.js benchmark](https://github.com/oven-sh/bun/tree/main/bench/bundle).

## Why bundle?

Bundlers solve several problems:- **Reducing HTTP requests.**A single package in- `node_modules`may consist of hundreds of files, and large applications may have dozens of such dependencies. Loading each of these files with a separate HTTP request becomes untenable, so bundlers convert your application source code into a smaller number of self-contained “bundles” that can be loaded with a single request.
- **Code transforms.**Modern apps are commonly built with languages or tools like TypeScript, JSX, and CSS modules, all of which must be converted into plain JavaScript and CSS before they can be consumed by a browser. The bundler is the natural place to configure these transformations.
- **Framework features.**Frameworks rely on bundler plugins & code transformations to implement common patterns like file-system routing, client-server code co-location (think- `getServerSideProps`or Remix loaders), and server components.
- **Full-stack Applications.**Bun’s bundler can handle both server and client code in a single command, enabling optimized production builds and single-file executables. With build-time HTML imports, you can bundle your entire application — frontend assets and backend server — into a single deployable unit.

The Bun bundler is not intended to replace 

`tsc` for typechecking or generating type declarations.## Basic example

Build your first bundle. You have the following two files, which implement a client-side rendered React app.`index.tsx` is the “entrypoint” to the application: the file the bundler starts from. Commonly, this is a script that performs some side effect, like starting a server or, in this case, initializing a React root. Because these files use TypeScript and JSX, the code must be bundled before it can be sent to the browser.
To create the bundle:
`entrypoints`, Bun generates a new bundle and writes it to the `./out` directory (as resolved from the current working directory). After running the build, the file system looks like this:
file system

`out/index.js` look something like this:
out/index.js

## Watch mode

Like the runtime and test runner, the bundler supports watch mode natively.terminal

## Content types

Like the Bun runtime, the bundler supports a range of file types by default. The following table lists the bundler’s standard “loaders”. See[loaders](/docs/bundler/loaders).

### Assets

If the bundler encounters an import with an unrecognized extension, it treats the imported file as an external file. The referenced file is copied as-is into`outdir`, and the import is resolved as a path to the file.
[and](#naming)

`naming`[.](#publicpath)

`publicPath`See 

[loaders](/docs/bundler/loaders)for more on the file loader.### Plugins

Plugins can override or extend the behavior described in this table. See[loaders](/docs/bundler/loaders).

## API

### entrypoints

Required An array of paths corresponding to the entrypoints of your application. Bun generates one bundle per entrypoint.- JavaScript
- CLI

build.ts

### files

A map of file paths to their contents for in-memory bundling: bundle virtual files that don’t exist on disk, or override the contents of files that do. This option is only available in the JavaScript API. File contents can be provided as a`string`, `Blob`, `TypedArray`, or `ArrayBuffer`.
#### Bundle entirely from memory

You can bundle code without any files on disk by providing all sources in`files`:
build.ts

`files` map, the current working directory is used as the root.
#### Override files on disk

In-memory files take priority over files on disk, so you can override specific files while keeping the rest of your codebase unchanged:build.ts

#### Mix disk and virtual files

Real files on disk can import virtual files, and virtual files can import real files:build.ts

### outdir

The directory where output files are written.- JavaScript
- CLI

build.ts

`outdir` is not passed to the JavaScript API, bundled code is not written to disk. Bundled files are returned in an array of `BuildArtifact` objects. These objects are Blobs with extra properties; see [Outputs](#outputs).

build.ts

`outdir` is set, the `path` property on a `BuildArtifact` is the absolute path it was written to.
### target

The intended execution environment for the bundle.- JavaScript
- CLI

build.ts

## browser

**Default.**For bundles that run in a browser. Prioritizes the

`"browser"` export condition when resolving imports.
Importing built-in modules like `node:events` or `node:path` works, but calling some functions, like `fs.readFile`,
does not.## bun

For bundles that run in the Bun runtime. In many cases, it isn’t necessary to bundle server-side code; you can directly execute the source code without modification. However, bundling your server code can reduce startup times and improve running performance. Use this target for full-stack applications with build-time HTML imports, where server and client code are bundled together.All bundles generated with 

`target: "bun"` are marked with a `// @bun` pragma, which tells the Bun runtime that there’s no need to re-transpile the file before execution.If any entrypoint contains a Bun shebang (`#!/usr/bin/env bun`), the bundler defaults to `target: "bun"` instead of `"browser"`.When using `target: "bun"` and `format: "cjs"` together, the `// @bun @bun-cjs` pragma is added and the CommonJS wrapper function is not compatible with Node.js.## node

For bundles that run in Node.js. Prioritizes the 

`"node"` export condition when resolving imports. Bun does not
polyfill the `Bun` global or the built-in `bun:*` modules.### format

Specifies the module format of the generated bundles. Bun defaults to`"esm"`, and provides experimental support for `"cjs"` and `"iife"`.
#### format: “esm” - ES Module

The default format. Supports ES Module syntax, including top-level await and`import.meta`.
- JavaScript
- CLI

build.ts

`format` to `"esm"` and load the bundle with a `<script type="module">` tag.
#### format: “cjs” - CommonJS

To build a CommonJS module, set`format` to `"cjs"`. When choosing `"cjs"`, the default target changes from `"browser"` (esm) to `"node"` (cjs). CommonJS modules transpiled with `format: "cjs"`, `target: "node"` run in both Bun and Node.js (assuming the APIs in use are supported by both).
- JavaScript
- CLI

build.ts

#### format: “iife” - IIFE

TODO: document IIFE once we support globalNames.`jsx`

Configures how JSX is compiled.
**Classic runtime example**(uses

`factory` and `fragment`):
**Automatic runtime example**(uses

`importSource`):
### splitting

Whether to enable code splitting.- JavaScript
- CLI

build.ts

`true`, the bundler enables code splitting. When multiple entrypoints import the same file or module, the bundler can split that shared code into a separate bundle, known as a **chunk**. Consider the following files:

`entry-a.ts` and `entry-b.ts` with code-splitting enabled:
- JavaScript
- CLI

build.ts

file system

`chunk-2fce6291bf86559d.js` file contains the shared code. To avoid collisions, the file name includes a content hash by default. Customize this with [.](#naming)

`naming`### plugins

A list of plugins to use during bundling.build.ts

[plugins](/docs/bundler/plugins).

### env

Controls how environment variables are handled during bundling. Internally, this uses`define` to inject environment variables into the bundle; `env` is a shorthand for specifying which ones.
#### env: “inline”

Injects environment variables into the bundled output by converting`process.env.FOO` references to string literals containing the actual environment variable values.
- JavaScript
- CLI

build.ts

input.js

output.js

#### env: “PUBLIC_*” (prefix)

Inlines environment variables matching the given prefix (the part before the`*` character), replacing `process.env.FOO` with the actual environment variable value. Use a prefix to inline public values, like public-facing URLs or client-side tokens, without injecting private credentials into output bundles.
- JavaScript
- CLI

build.ts

terminal

index.tsx

output.js

#### env: “disable”

Disables environment variable injection entirely.### sourcemap

Specifies the type of sourcemap to generate.- JavaScript
- CLI

build.ts

The associated 

`*.js.map` sourcemap is a JSON file containing an equivalent `debugId` property.
### minify

Whether to enable minification. Default`false`.
To enable all minification options:
- JavaScript
- CLI

build.ts

- JavaScript
- CLI

build.ts

### external

A list of import paths to consider external. Defaults to`[]`.
- JavaScript
- CLI

build.ts

index.tsx

`index.tsx` would generate a bundle containing the entire source code of the “zod” package. To leave the import statement as-is instead, mark it as external:
- JavaScript
- CLI

build.ts

out/index.js

`*`:
- JavaScript
- CLI

build.ts

### packages

Controls whether package dependencies are included in the bundle. Possible values:`bundle` (default), `external`. Bun treats any import whose path does not start with `.`, `..`, or `/` as a package.
- JavaScript
- CLI

build.ts

### naming

Customizes the generated file names. Defaults to`./[dir]/[name].[ext]`.
- JavaScript
- CLI

build.ts

file system

file system

`naming` field customizes the names and locations of the generated files. It accepts a template string, used for all bundles corresponding to entrypoints, in which the following tokens are replaced with their values:
- `[name]`- The name of the entrypoint file, without the extension.
- `[ext]`- The extension of the generated bundle.
- `[hash]`- A hash of the bundle contents.
- `[dir]`- The relative path from the project root to the parent directory of the source file.

Combine these tokens to create a template string. For instance, to include the hash in the generated bundle names:

- JavaScript
- CLI

build.ts

file system

`naming` field, it is used only for bundles that correspond to entrypoints. The names of chunks and copied assets are not affected. In the JavaScript API, you can specify a separate template string for each type of generated file.
- JavaScript
- CLI

build.ts

### root

The root directory of the project.- JavaScript
- CLI

build.ts

file system

`pages` directory:
- JavaScript
- CLI

file system

`pages` directory is the first common ancestor of the entrypoint files, it is considered the project root, so the generated bundles live at the top level of the `out` directory; there is no `out/pages` directory.
Override this by specifying the `root` option:
- JavaScript
- CLI

`.` as `root`, the generated file structure looks like this:
### publicPath

A prefix added to any import paths in bundled code. In many cases, generated bundles contain no import statements; the goal of bundling is to combine all of the code into a single file. In a few cases, though, the generated bundles contain import statements:- **Asset imports**— When importing an unrecognized file type like- `*.svg`, the bundler defers to the file loader, which copies the file into- `outdir`as is. The import is converted into a variable.
- **External modules**— Files and modules marked as external are not included in the bundle. Instead, the import statement is left in the final bundle.
- **Chunking.**When- `splitting`is enabled, the bundler may generate separate “chunk” files that represent code that is shared among multiple entrypoints.

`publicPath` prefixes all file paths with the specified value.
- JavaScript
- CLI

build.ts

out/index.js

### define

A map of global identifiers to be replaced at build time. Keys of this object are identifier names, and values are JSON strings that are inlined.- JavaScript
- CLI

build.ts

### loader

A map of file extensions to built-in loader names. Use this to customize how certain files are loaded.- JavaScript
- CLI

build.ts

### banner

A banner added to the final bundle. This can be a directive like`"use client"` for React, or a comment block such as a license.
- JavaScript
- CLI

build.ts

### footer

A footer added to the final bundle. This can be a comment block for a license or a fun easter egg.- JavaScript
- CLI

build.ts

### drop

Removes function calls from a bundle. For example,`--drop=console` removes all calls to `console.log`. Arguments to dropped calls are also removed, even if they have side effects. Dropping `debugger` removes all `debugger` statements.
- JavaScript
- CLI

build.ts

### features

Enable compile-time feature flags for dead code elimination: conditionally include or exclude code paths at bundle time using`import { feature } from "bun:bundle"`.
app.ts

- JavaScript
- CLI

build.ts

`feature()` function is replaced with `true` or `false` at bundle time. Combined with minification, unreachable code is eliminated:
Input

Output (with --feature PREMIUM --minify)

Output (without --feature PREMIUM, with --minify)

**Key behaviors:**

- `feature()`requires a string literal argument — dynamic values are not supported
- The `bun:bundle`import is completely removed from the output
- Works with `bun build`,`bun run`, and`bun test`
- Multiple flags can be enabled: `--feature FLAG_A --feature FLAG_B`
- For type safety, augment the `Registry`interface to restrict`feature()`to known flags

**Use cases:**

- Platform-specific code (`feature("SERVER")`vs`feature("CLIENT")`)
- Environment-based features (`feature("DEVELOPMENT")`)
- Gradual feature rollouts
- A/B testing variants
- Paid tier features

**Type safety:**By default,

`feature()` accepts any string. To get autocomplete and catch typos at compile time, create an `env.d.ts` file (or add to an existing `.d.ts`) and augment the `Registry` interface:
env.d.ts

`tsconfig.json` (for example, `"include": ["src", "env.d.ts"]`). Now `feature()` only accepts those flags, and invalid strings like `feature("TYPO")` become type errors.
### optimizeImports

Skip parsing unused submodules of barrel files (re-export index files). When you import only a few named exports from a large library, normally the bundler parses every file the barrel re-exports. With`optimizeImports`, only the submodules you use are parsed.
build.ts

`import { Button } from 'antd'` normally parses all ~3000 modules that `antd/index.js` re-exports. With `optimizeImports: ['antd']`, only the `Button` submodule is parsed.
This works for **pure barrel files**— files where every named export is a re-export (

`export { X } from './x'`). If a barrel file has any local exports (`export const foo = ...`), or if any importer uses `import *`, all submodules are loaded.
`export *` re-exports are always loaded (never deferred) to avoid circular resolution issues. Only named re-exports (`export { X } from './x'`) that aren’t used by any importer are deferred.
**Automatic mode:**Packages with

`"sideEffects": false` in their `package.json` get barrel optimization automatically — no `optimizeImports` config needed. Use `optimizeImports` for packages that don’t have this field.
**Plugins:**Resolve and load plugins work with barrel optimization. Deferred submodules go through the plugin pipeline when they are eventually loaded.

### metafile

Generate metadata about the build in a structured format. The metafile describes every input and output file: sizes, imports, and exports. Use it for:- **Bundle analysis**: Understand what’s contributing to bundle size
- **Visualization**: Feed into tools like- [esbuild’s bundle analyzer](https://esbuild.github.io/analyze/)
- **Dependency tracking**: See the full import graph of your application
- **CI integration**: Track bundle size changes over time

- JavaScript
- CLI

build.ts

#### Markdown metafile

Use`--metafile-md` to generate a markdown metafile, which is LLM-friendly and readable in the terminal:
terminal

`--metafile` and `--metafile-md` can be used together:
terminal

`metafile` option formats

In the JavaScript API, `metafile` accepts several forms:
build.ts

## Outputs

The`Bun.build` function returns a `Promise<BuildOutput>`, defined as:
build.ts

`outputs` array contains all the files generated by the build. Each artifact implements the Blob interface.
build.ts

Similar to 

`BunFile`, `BuildArtifact` objects can be passed directly into `new Response()`.
build.ts

`BuildArtifact` objects to make debugging easier.
## Bytecode

The`bytecode: boolean` option generates bytecode for any JavaScript/TypeScript entrypoints, which can greatly improve startup times for large applications. Requires `"target": "bun"` and a matching version of Bun.
- **CommonJS**: Works with or without- `compile: true`. Generates a- `.jsc`file alongside each entrypoint.
- **ESM**: Requires- `compile: true`. Bytecode and module metadata are embedded in the standalone executable.

`format`, bytecode defaults to CommonJS.
- JavaScript
- CLI

build.ts

## Executables

Bun supports “compiling” a JavaScript/TypeScript entrypoint into a standalone executable. This executable contains a copy of the Bun binary.terminal

[standalone executables](/docs/bundler/executables).

## Logs and errors

On failure,`Bun.build` returns a rejected promise with an `AggregateError`. Log it to the console to pretty-print the error list, or read it programmatically with a try/catch block.
build.ts

`Bun.build` call instead.
Each item in `error.errors` is an instance of `BuildMessage` or `ResolveMessage` (subclasses of `Error`), containing detailed information for each error.
build.ts

`logs` property, which contains bundler warnings and info messages.
build.ts

## Reference

Typescript Definitions

## CLI Usage

### General Configuration

Set 

`NODE_ENV=production` and enable minificationUse a bytecode cache when compiling

Intended execution environment for the bundle. One of 

`browser`, `bun`, or `node`Pass custom resolution conditions

Inline environment variables into the bundle as 

`process.env.$`. To inline variables matching a
prefix, use a glob like `FOO_PUBLIC_*`### Output & File Handling

Output directory (used when building multiple entry points)

Write output to a specific file

Generate source maps. One of 

`linked`, `inline`, `external`, or `none`Add a banner to the output (e.g. 

`“use client”` for React Server Components)Add a footer to the output (e.g. 

`// built with bun!`)Module format of the output bundle. One of 

`esm`, `cjs`, or `iife`. Defaults to
`cjs` when `—bytecode` is used.### File Naming

Customize entry point filenames

Customize chunk filenames

Customize asset filenames

### Bundling Options

Root directory used when bundling multiple entry points

Enable code splitting for shared modules

Prefix to be added to import paths in bundled code

Exclude modules from the bundle (supports wildcards). Alias: 

`-e`How to treat dependencies: 

`external` or `bundle`Transpile only — do not bundle

Chunk CSS files together to reduce duplication (only when multiple entry points import CSS)

### Minification & Optimization

Re-emit Dead Code Elimination annotations. Disabled when 

`—minify-whitespace` is usedEnable all minification options

Minify syntax and inline constants

Minify whitespace

Minify variable and function identifiers

Preserve original function and class names when minifying

### Development Features

Rebuild automatically when files change

Don’t clear the terminal when rebuilding with 

`—watch`Enable React Fast Refresh transform (for development testing)

Run the React Compiler over 

`.jsx`/`.tsx` files, automatically memoizing components and hooks. Output mode is derived
from `--target` (`browser` → client, `bun`/`node` → ssr). Experimental.### Standalone Executables

Generate a standalone Bun executable containing the bundle

Prepend arguments to the standalone executable’s 

`execArgv`### Windows Executable Details

Prevent a console window from opening when running a compiled Windows executable

Set an icon for the Windows executable

Set the Windows executable product name

Set the Windows executable company name

Set the Windows executable version (e.g. 

`1.2.3.4`)Set the Windows executable description

Set the Windows executable copyright notice

### Experimental & App Building

**(EXPERIMENTAL)**Build a web app for production using Bun Bake

**(EXPERIMENTAL)**Enable React Server Components

When 

`—app` is set, dump all server files to disk even for static buildsWhen 

`—app` is set, disable all minification

# Citations

1. Source page: https://bun.sh/docs/bundler
