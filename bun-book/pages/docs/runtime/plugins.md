---
type: Web Page
title: Plugins - Bun
description: Universal plugin API for extending Bun's runtime and bundler
resource: https://bun.sh/docs/runtime/plugins
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

*runtime*and the

*bundler*. Plugins intercept imports and perform custom loading logic, like reading files or transpiling code. Use them to add support for additional file types, such as

`.scss` or `.yaml`. In Bun’s bundler, plugins can implement framework-level features like CSS extraction, macros, and client-server code co-location.
## Lifecycle hooks

Plugins register callbacks that run at various points in the lifecycle of a bundle:- `onStart()`
- `onResolve()`
- `onLoad()`
- `onBeforeParse()`

### Reference

A rough overview of the types (see Bun’s`bun.d.ts` for the full type definitions):
Plugin Types

## Usage

A plugin is defined as a JavaScript object containing a`name` property and a `setup` function.
myPlugin.ts

`plugins` array when calling `Bun.build`.
index.ts

## Plugin lifecycle

### Namespaces

`onLoad` and `onResolve` accept an optional `namespace` string. Every module has a namespace, which prefixes the import in transpiled code; for instance, a loader with a `filter: /\.yaml$/` and `namespace: "yaml:"` transforms an import from `./myfile.yaml` into `yaml:./myfile.yaml`.
The default namespace is `"file"` and you don’t need to specify it: `import myModule from "./my-module.ts"` is the same as `import myModule from "file:./my-module.ts"`.
Other common namespaces are:
- `"bun"`: for Bun-specific modules (- `"bun:test"`,- `"bun:sqlite"`)
- `"node"`: for Node.js modules (- `"node:fs"`,- `"node:path"`)

`onStart`

index.ts

`Promise`. After the bundle process has initialized, the bundler waits until all `onStart()` callbacks have completed before continuing.
For example:
index.ts

`onStart()` callbacks to complete: the first sleeps for 10 seconds and the second writes the bundle time to a file.
`onStart()` callbacks (like every other lifecycle callback) cannot modify the `build.config` object. To mutate `build.config`, do so directly in the `setup()` function.
`onResolve`

`onResolve()` lifecycle callback customizes how a module is resolved.
The first argument to `onResolve()` is an object with a `filter` and [property. The filter is a regular expression run on the import string. Together they determine which modules your custom resolution logic applies to. The second argument to](#what-is-a-namespace)

`namespace``onResolve()` is a callback that runs for each module import Bun finds that matches the `filter` and `namespace` defined in the first argument.
The callback receives the *path*to the matching module and can return a

*new path*for it. Bun reads the contents of the

*new path*and parses it as a module. For example, redirecting all imports to

`images/` to `./public/images/`:
index.ts

`onLoad`

`onLoad()` lifecycle callback to modify the *contents*of a module before Bun reads and parses it. Like

`onResolve()`, the first argument to `onLoad()` filters which modules this invocation of `onLoad()` applies to.
The second argument to `onLoad()` is a callback that runs for each matching module *before*Bun loads its contents into memory. The callback receives the matching module’s

*path*, its

*namespace*, the default

*loader*for that file, and a

*defer*function. The callback can return a new

`contents` string for the module as well as a new `loader`.
For example:
index.ts

`import env from "env"` into a JavaScript module that exports the current environment variables.
`.defer()`

The `onLoad` callback receives a `defer` function, which returns a `Promise` that resolves once all *other*modules have been loaded. Await it when a module’s contents depend on other modules.

##### Example: tracking and reporting unused exports

index.ts

`.defer()` function can only be called once per `onLoad` callback.
## Native plugins

Bun’s bundler is fast partly because it is written in native code and uses multiple threads to load and parse modules in parallel. Plugins written in JavaScript cannot take advantage of this, because JavaScript itself is single-threaded. Native plugins are written as[NAPI](/docs/runtime/node-api)modules and can run on multiple threads, so they run much faster than JavaScript plugins. They can also skip unnecessary work such as the UTF-8 -> UTF-16 conversion needed to pass strings to JavaScript. The following lifecycle hooks are available to native plugins:

- `onBeforeParse()`

### Creating a native plugin in Rust

terminal

terminal

`lib.rs`, use the `bun_native_plugin::bun` proc macro to define a function that implements your native plugin.
Here’s an example implementing the `onBeforeParse` hook:
lib.rs

`Bun.build()`:

# Citations

1. Source page: https://bun.sh/docs/runtime/plugins
