---
type: Web Page
title: Environment Variables - Bun
description: Read and configure environment variables in Bun, including automatic
  .env file support
resource: https://bun.sh/docs/runtime/environment-variables
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`.env` files automatically and provides idiomatic ways to read and write your environment variables programmatically. You can also configure parts of Bun’s runtime behavior with Bun-specific environment variables.
## Setting environment variables

Bun reads the following files automatically (listed in order of increasing precedence).- `.env`
- `.env.production`,- `.env.development`,- `.env.test`(depending on the value of- `NODE_ENV`)
- `.env.local`

.env

Cross-platform solution with Windows

Cross-platform solution with Windows

For a cross-platform solution, use Bun Shell, for example through On Windows, 

`bun exec`.`package.json` scripts called with `bun run` automatically use the **Bun Shell**, so the following is also cross-platform.package.json

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
`AWESOME` property to `process.env` and `Bun.env`.
## Configuring Bun

Bun reads these environment variables to configure aspects of its behavior.| Name | Description | 
|---|---|
| `NODE_TLS_REJECT_UNAUTHORIZED` | `NODE_TLS_REJECT_UNAUTHORIZED=0`disables SSL certificate validation. Useful for testing and debugging, but be very hesitant to use it in production. Node.js introduced this variable; Bun keeps the name for compatibility. | 
| `BUN_CONFIG_VERBOSE_FETCH` | If `BUN_CONFIG_VERBOSE_FETCH=curl`, then fetch requests log the URL, method, request headers and response headers to the console. This also works with`node:http`.`BUN_CONFIG_VERBOSE_FETCH=1`is equivalent to`BUN_CONFIG_VERBOSE_FETCH=curl`except without the`curl`output. | 
| `BUN_RUNTIME_TRANSPILER_CACHE_PATH` | The runtime transpiler caches the transpiled output of source files larger than 50 KB, which makes CLIs using Bun load faster. If `BUN_RUNTIME_TRANSPILER_CACHE_PATH`is set, the cache is written to that directory. If it is set to an empty string or the string`"0"`, caching is disabled. If it is unset, the cache is written to the platform-specific cache directory. | 
| `TMPDIR` | Bun occasionally requires a directory to store intermediate assets during bundling or other operations. If unset, defaults to the platform-specific temporary directory: `/tmp`on Linux,`/private/tmp`on macOS. | 
| `NO_COLOR` | If `NO_COLOR=1`, then ANSI color output is disabled. | 
| `FORCE_COLOR` | If `FORCE_COLOR=1`, then ANSI color output is forced on, even if`NO_COLOR`is set. | 
| `BUN_CONFIG_MAX_HTTP_REQUESTS` | Sets the maximum number of concurrent HTTP requests sent by fetch and `bun install`. Defaults to`256`. Lower it if you run into rate limits or connection issues. | 
| `BUN_CONFIG_NO_CLEAR_TERMINAL_ON_RELOAD` | If `BUN_CONFIG_NO_CLEAR_TERMINAL_ON_RELOAD=true`, then`bun --watch`does not clear the console on reload | 
| `DO_NOT_TRACK` | Disable uploading crash reports to `bun.report`on crash. On macOS & Windows, crash report uploads are enabled by default. Other telemetry is not sent, though we plan to add some. If`DO_NOT_TRACK=1`, then auto-uploading crash reports and telemetry are both disabled. | 
| `BUN_OPTIONS` | Prepends command-line arguments to any Bun execution. For example, `BUN_OPTIONS="--hot"`makes`bun run dev`behave like`bun --hot run dev`. | 

## Runtime transpiler caching

For files larger than 50 KB, Bun caches transpiled output into`$BUN_RUNTIME_TRANSPILER_CACHE_PATH` or the platform-specific cache directory. This makes CLIs using Bun load faster.
The cache is global and shared across all projects, and it is content-addressable, so it never contains duplicate entries. It is safe to delete at any time, even while a Bun process is running.
Disable this cache when using ephemeral filesystems like Docker. Bun’s Docker images disable it automatically.
### Disable the runtime transpiler cache

To disable the runtime transpiler cache, set`BUN_RUNTIME_TRANSPILER_CACHE_PATH` to an empty string or the string `"0"`.
### What does it cache?

It caches:- The transpiled output of source files larger than 50 KB.
- The sourcemap for the transpiled output of the file

`.pile` extension.

# Citations

1. Source page: https://bun.sh/docs/runtime/environment-variables
