---
type: Web Page
title: Environment Variables - Bun
description: Read and configure environment variables in Bun, including automatic
  .env file support
resource: https://bun.sh/docs/runtime/environment-variables
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

`.env` files automatically and provides idiomatic ways to read and write your environment variables programmatically. You can also configure parts of Bun’s runtime behavior with Bun-specific environment variables.
## Setting environment variables

Bun reads the following files automatically (listed in order of increasing precedence).- `.env`
- `.env.production`,- `.env.development`,- `.env.test`(depending on the value of- `NODE_ENV`)
- `.env.local`

.env

Cross-platform solution with Windows

Cross-platform solution with Windows

For a cross-platform solution, use On Windows, 

[Bun Shell](/docs/runtime/shell), for example through`bun exec`.`package.json` scripts called with `bun run` automatically use the **Bun Shell**, so the following is also cross-platform.package.json

`process.env`.
## Manually specifying `.env` files

The `--env-file` flag overrides which `.env` files Bun loads. It works both when running a file with `bun` and when running `package.json` scripts.
## Disabling automatic `.env` loading

Use `--no-env-file` to disable Bun’s automatic `.env` file loading, for example in production or CI/CD pipelines where you want to rely solely on system environment variables.
`bunfig.toml`:
bunfig.toml

`--env-file` still load even when default loading is disabled.
## Quotation marks

Bun supports double quotes, single quotes, and template literal backticks:.env

### Expansion

Bun automatically*expands*environment variables, so you can reference previously-defined variables.

.env

.env

`$` with a backslash.
.env

`dotenv`

Bun reads `.env` files automatically, so `dotenv` and `dotenv-expand` are unnecessary.
## Reading environment variables

Read the current environment variables from`process.env`.
`Bun.env` and `import.meta.env`, both aliases of `process.env`.
`bun --print process.env`.
## TypeScript

In TypeScript, all properties of`process.env` are typed as `string | undefined`.
[interface merging](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#merging-interfaces).

`AWESOME` property to `process.env` and `Bun.env`.
## Configuring Bun

Bun reads these environment variables to configure aspects of its behavior.## Runtime transpiler caching

For files larger than 4 KB, Bun caches transpiled output into`$BUN_RUNTIME_TRANSPILER_CACHE_PATH` or the platform-specific cache directory. This makes CLIs using Bun load faster.
The cache is global and shared across all projects, and it is content-addressable, so it never contains duplicate entries. It is safe to delete at any time, even while a Bun process is running.
Disable this cache when using ephemeral filesystems like Docker. Bun’s Docker images disable it automatically.
### Disable the runtime transpiler cache

To disable the runtime transpiler cache, set`BUN_RUNTIME_TRANSPILER_CACHE_PATH` to an empty string or the string `"0"`.
### What does it cache?

It caches:- The transpiled output of source files larger than 4 KB.
- The sourcemap for the transpiled output of the file

`.pile` extension.

# Citations

1. Source page: https://bun.sh/docs/runtime/environment-variables
