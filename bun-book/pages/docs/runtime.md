---
type: Web Page
title: Bun Runtime - Bun
description: Execute JavaScript/TypeScript files, package.json scripts, and executable
  packages with Bun's fast runtime.
resource: https://bun.sh/docs/runtime
timestamp: '2026-08-03T08:59:43.078871+00:00'
---

[JavaScriptCore engine](https://developer.apple.com/documentation/javascriptcore), developed by Apple for Safari. It usually starts and runs faster than V8, the engine used by Node.js and Chromium-based browsers. Bun’s transpiler and runtime are written in Rust. On Linux, Bun starts

[4x faster](https://twitter.com/jarredsumner/status/1499225725492076544)than Node.js.

The benchmark runs a Hello World script on Linux.

## Run a file

Use`bun run` to execute a source file.
terminal

[transpiler](/docs/runtime/transpiler)before running it.

terminal

`run` keyword and use the “naked” command; it behaves identically.
terminal

### `--watch`

To run a file in watch mode, use the `--watch` flag.
terminal

When using Flags at the end of the command are ignored by 

`bun run`, put Bun flags like `--watch` immediately after `bun`.`bun` and passed through to the `"dev"` script itself.
## Run a `package.json` script

Compare to 

`npm run <script>` or `yarn <script>``package.json` can define named `"scripts"` that correspond to shell commands.
package.json

`bun run <script>` to execute these scripts.
terminal

`bash`, `sh`, `zsh`. On Windows, it uses the [Bun Shell](/docs/runtime/shell)to support bash-like syntax and many common commands.

⚡️ The startup time for 

`npm run` on Linux is roughly 170ms; with Bun it is `6ms`.`bun <script>`. If a built-in `bun` command has the same name, the built-in command takes precedence; use the explicit `bun run <script>` to run your package script instead.
terminal

`bun run` without any arguments.
terminal

`bun run clean` runs `preclean` and `postclean`, if defined. If the `pre<script>` fails, Bun does not run the script itself.
### `--bun`

It’s common for `package.json` scripts to reference locally-installed CLIs like `vite` or `next`. These CLIs are often JavaScript files marked with a [shebang](<https://en.wikipedia.org/wiki/Shebang_(Unix)>)to indicate that they should be executed with

`node`.
cli.js

`node`. The `--bun` flag overrides it: the CLI runs with Bun instead of Node.js.
terminal

### Filtering

In a monorepo, the`--filter` argument runs a script in many packages at once.
`bun run --filter <name_pattern> <script>` executes `<script>` in every package whose name matches `<name_pattern>`.
For example, if you have subdirectories containing packages named `foo`, `bar` and `baz`, running
terminal

`<script>` in both `bar` and `baz`, but not in `foo`.
See [.](/docs/pm/filter#running-scripts-with-filter)

`--filter`
## `bun run -` to pipe code from stdin

`bun run -` reads JavaScript, TypeScript, TSX, or JSX from stdin and executes it without writing to a temporary file first.
terminal

`bun run -` to redirect files into Bun. For example, to run a `.js` file as if it were a `.ts` file:
terminal

`bun run -` treats all input as TypeScript with JSX support.
## `bun run --console-depth`

Control the depth of object inspection in console output with the `--console-depth` flag.
terminal

`--console-depth` sets how deeply nested objects are displayed in `console.log()` output. The default depth is `2`. Higher values show more nested properties but may produce verbose output for complex objects.
console.ts

## `bun run --smol`

In memory-constrained environments, use the `--smol` flag to reduce memory usage at a cost to performance.
terminal

`--smol` makes the garbage collector run more frequently, which can slow down execution. Bun adjusts the garbage collector’s heap size based on the available memory (accounting for cgroups and other memory limits) with and without the `--smol` flag, so the flag is mostly useful when you want the heap to grow more slowly.
## Resolution order

Absolute paths and paths starting with`./` or `.\\` are always executed as source files. Unless you use `bun run`, a name with an allowed extension resolves to the file rather than a `package.json` script.
When a `package.json` script and a file have the same name, `bun run` prefers the script. The full resolution order is:
1. `package.json` scripts:`bun run build`
2. Source files: `bun run src/main.js`
3. Binaries from project packages: `bun add eslint && bun run eslint`
4. (`bun run` only) System commands:`bun run ls`

# CLI Usage

### General Execution Options

boolean

Don’t print the script command

boolean

Exit without an error if the entrypoint does not exist

string

Evaluate argument as a script. Alias: 

`-e`
string

Evaluate argument as a script and print the result. Alias: 

`-p`
boolean

Display this menu and exit. Alias: 

`-h`
### Workspace Management

number

default:"10"

Number of lines of script output shown when using —filter (default: 10). Set to 0 to show all lines

string

Run a script in all workspace packages matching the pattern. Alias: 

`-F`
boolean

Run a script in all workspace packages (from the 

`workspaces` field in `package.json`)
boolean

Run multiple scripts or workspace scripts concurrently with prefixed output

boolean

Run multiple scripts or workspace scripts one after another with prefixed output

boolean

When using 

`—parallel` or `—sequential`, continue running other scripts when one fails
### Runtime & Process Control

boolean

Force a script or package to use Bun’s runtime instead of Node.js (via symlinking node). Alias: 

`-b`
string

Control the shell used for 

`package.json` scripts. Supports either `bun` or `system`
boolean

Open the Node.js-compatible REPL (

`node:repl`). When combined with `-e`, starts the REPL and
then evaluates the script. Under `—interactive`, `-e` is raw JavaScript (matching
`node -i -e`); use `bun repl` for TypeScript. Distinct from `bun repl`, which is
Bun’s native REPL.
boolean

Use less memory, but run garbage collection more often

boolean

Expose 

`gc()` on the global object. Has no effect on `Bun.gc()`
boolean

Suppress all reporting of the custom deprecation

boolean

Determine whether deprecation warnings result in errors

string

Set the process title

boolean

Force 

`Buffer.allocUnsafe(size)` to be zero-filled
boolean

Throw an error if 

`process.dlopen` is called, and disable export condition `node-addons`
string

One of 

`strict`, `throw`, `warn`, `none`, or
`warn-with-error-code`
number

default:"2"

Set the default depth for 

`console.log` object inspection (default: 2)
### Development Workflow

boolean

Automatically restart the process on file change

boolean

Enable auto reload in the Bun runtime, test runner, or bundler

boolean

Disable clearing the terminal screen on reload when —hot or —watch is enabled

### Debugging

string

Activate Bun’s debugger

string

Activate Bun’s debugger, wait for a connection before executing

string

Activate Bun’s debugger, set breakpoint on first line of code and wait

### Dependency & Module Resolution

string

Import a module before other modules are loaded. Alias: 

`-r`
string

Alias of —preload, for Node.js compatibility

string

Alias of —preload, for Node.js compatibility

boolean

Disable auto install in the Bun runtime

string

default:"auto"

Configure auto-install behavior. One of 

`auto` (default, auto-installs when no node_modules),
`fallback` (missing packages only), `force` (always)
boolean

Auto-install dependencies during execution. Equivalent to —install=fallback

boolean

Skip staleness checks for packages in the Bun runtime and resolve from disk

boolean

Use the latest matching versions of packages in the Bun runtime, always checking npm

string

Pass custom conditions to resolve

string

Main fields to lookup in 

`package.json`. Defaults to —target dependent
boolean

Preserve symlinks when resolving files

boolean

Preserve symlinks when resolving the main entry point

string

default:".tsx,.ts,.jsx,.js,.json"

Defaults to: 

`.tsx,.ts,.jsx,.js,.json`
### Transpilation & Language Features

string

Specify custom 

`tsconfig.json`. Default `$cwd/tsconfig.json`
string

Substitute K:V while parsing, e.g. 

`—define process.env.NODE_ENV:“development”`. Values are parsed as
JSON. Alias: `-d`
string

Remove function calls, e.g. 

`—drop=console` removes all `console.*` calls
string

Parse files with 

`.ext:loader`, e.g. `—loader .js:jsx`. Valid loaders: `js`,
`jsx`, `ts`, `tsx`, `json`, `toml`, `text`,
`file`, `wasm`, `napi`. Alias: `-l`
boolean

Disable macros from being executed in the bundler, transpiler and runtime

string

Changes the function called when compiling JSX elements using the classic JSX runtime

string

Changes the function called when compiling JSX fragments

string

default:"react"

Declares the module specifier used to import the jsx and jsxs factory functions. Default: 

`react`
string

default:"automatic"

`automatic` (default) or `classic`
boolean

Treat JSX elements as having side effects (disable pure annotations)

boolean

Ignore tree-shaking annotations such as 

`@`**PURE**
### Networking & Security

number

Set the default port for 

`Bun.serve`
string

Preconnect to a URL while code is loading

number

default:"16384"

Set the maximum size of HTTP headers in bytes. Default is 16KiB

string

default:"verbatim"

Set the default order of DNS lookup results. Valid orders: 

`verbatim` (default), `ipv4first`,
`ipv6first`
boolean

Use the system’s trusted certificate authorities

boolean

Use OpenSSL’s default CA store

boolean

Use bundled CA store

boolean

Preconnect to 

`$REDIS_URL` at startup
boolean

Preconnect to PostgreSQL at startup

string

Set the default User-Agent header for HTTP requests

### Global Configuration & Context

string

Load environment variables from the specified file(s)

string

Absolute path to resolve files & entrypoints from. This just changes the process’ cwd

string

Specify path to Bun config file. Default 

`$cwd/bunfig.toml`. Alias: `-c`

# Citations

1. Source page: https://bun.sh/docs/runtime
