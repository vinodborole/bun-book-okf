---
type: Web Page
title: bunx | Bun Docs
description: Run packages from npm
resource: https://bun.sh/docs/pm/bunx
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# bunx

Run packages from npm

`bunx` is an alias for `bun x`. The `bunx` CLI is auto-installed when you install `bun`.
Use `bunx` to auto-install and run packages from `npm`. It's Bun's equivalent of `npx` or `yarn dlx`.

`bunx cowsay "Hello world!"`
⚡️ **Speed** — With Bun's fast startup times, `bunx` is [roughly 100x
faster](https://twitter.com/jarredsumner/status/1606163655527059458) than `npx` for locally installed packages.

Packages can declare executables in the `"bin"` field of their `package.json`. These are known as *package executables* or *package binaries*.

```
{
  // ... other fields
  "name": "my-cli",
  "bin": {
    "my-cli": "dist/index.js"
  }
}
```
These executables are commonly plain JavaScript files marked with a [shebang line](<https://en.wikipedia.org/wiki/Shebang_(Unix)>) naming the program that should run them. The following file runs with `node`.

```
#!/usr/bin/env node
console.log("Hello world!");
```
Run these executables with `bunx`:

`bunx my-cli`
As with `npx`, `bunx` checks for a locally installed package first, then falls back to auto-installing it from `npm`. `bunx` stores installed packages in Bun's [global cache](/docs/pm/global-cache) for future use.

## Arguments and flags

To pass additional command-line flags and arguments through to the executable, place them after the executable name.

`bunx my-cli --foo bar`
## Shebangs

By default, Bun respects shebangs. If an executable is marked with `#!/usr/bin/env node`, Bun spins up a `node` process to execute the file. To run the executable with Bun's runtime instead, pass the `--bun` flag.

`bunx --bun my-cli`
The `--bun` flag must occur *before* the executable name. `bunx` passes flags that appear *after* the name through to the executable.

```
bunx --bun my-cli # good
bunx my-cli --bun # bad
```
## Package flag

**`--package <pkg>` or `-p <pkg>`** - Run a binary from a specific package. Useful when the binary name differs from the package name:

```
bunx -p renovate renovate-config-validator
bunx --package @angular/cli ng
```
To force a script to always run with Bun, give it a `bun` shebang.

`#!/usr/bin/env bun`
## Usage

`bunx [flags] <package>[@version] [flags and arguments for the package]`
Execute an npm package executable (CLI). If the package isn't installed in `node_modules`, Bun installs it into a global shared cache.

### Flags

Force the command to run with Bun instead of Node.js, even if the executable contains a Node shebang (```
#!/usr/bin/env
  node
```
)

Specify package to install when binary name differs from package name

Skip installation if package is not already installed

Enable verbose output during installation

Suppress output during installation

### Examples

```
# Run Prisma migrations
bunx prisma migrate
# Format a file with Prettier
bunx prettier foo.js
# Run a specific version of a package
bunx uglify-js@3.14.0 app.js
# Use --package when binary name differs from package name
bunx -p @angular/cli ng new my-app
# Force running with Bun instead of Node.js, even if the executable contains a Node shebang
bunx --bun vite dev foo.js
```

# Citations

1. Source page: https://bun.sh/docs/pm/bunx
