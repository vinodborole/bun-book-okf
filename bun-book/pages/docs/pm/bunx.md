---
type: Web Page
title: bunx - Bun
description: Run packages from npm
resource: https://bun.sh/docs/pm/bunx
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`bunx` is an alias for `bun x`. The `bunx` CLI is auto-installed when you install `bun`.`bunx` to auto-install and run packages from `npm`. It’s Bun’s equivalent of `npx` or `yarn dlx`.
terminal

⚡️ 

**Speed**— With Bun’s fast startup times,`bunx` is [roughly 100x faster](https://twitter.com/jarredsumner/status/1606163655527059458)than`npx` for locally installed packages.`"bin"` field of their `package.json`. These are known as *package executables*or

*package binaries*.

package.json

[shebang line](https://en.wikipedia.org/wiki/Shebang_(Unix))naming the program that should run them. The following file runs with

`node`.
dist/index.js

`bunx`:
terminal

`npx`, `bunx` checks for a locally installed package first, then falls back to auto-installing it from `npm`. Installed packages are stored in Bun’s [global cache](/docs/pm/global-cache)for future use.

## Arguments and flags

To pass additional command-line flags and arguments through to the executable, place them after the executable name.terminal

## Shebangs

By default, Bun respects shebangs. If an executable is marked with`#!/usr/bin/env node`, Bun spins up a `node` process to execute the file. To run the executable with Bun’s runtime instead, pass the `--bun` flag.
terminal

`--bun` flag must occur *before*the executable name. Flags that appear

*after*the name are passed through to the executable.

terminal

## Package flag

**- Run a binary from a specific package. Useful when the binary name differs from the package name:**

`--package <pkg>` or `-p <pkg>`terminal

`bun` shebang.
dist/index.js

## Usage

`node_modules`, Bun installs it into a global shared cache.
### Flags

Force the command to run with Bun instead of Node.js, even if the executable contains a Node shebang (

`#!/usr/bin/env     node`)Specify package to install when binary name differs from package name

Skip installation if package is not already installed

Enable verbose output during installation

Suppress output during installation

### Examples

terminal

# Citations

1. Source page: https://bun.sh/docs/pm/bunx
