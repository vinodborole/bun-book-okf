---
type: Web Page
title: bun install - Bun
description: Install packages with Bun's fast package manager
resource: https://bun.sh/docs/pm/cli/install
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

## Basic Usage

terminal

`bun` CLI contains a Node.js-compatible package manager designed to be a dramatically faster replacement for `npm`, `yarn`, and `pnpm`. It’s a standalone tool that works in existing Node.js projects; if your project has a `package.json`, you can use `bun install`.
**⚡️ 25x faster**— Switch from

`npm install` to `bun install` in any Node.js project to make your installations up to 25x faster.terminal

`bun install`:
- **Installs**all- `dependencies`,- `devDependencies`, and- `optionalDependencies`. Bun installs- `peerDependencies`by default.
- **Runs**your project’s- `{pre|post}install`and- `{pre|post}prepare`scripts at the appropriate time. For security reasons Bun- *does not execute*lifecycle scripts of installed dependencies.
- **Writes**a- `bun.lock`lockfile to the project root.

## Logging

To modify logging verbosity:terminal

## Lifecycle scripts

Unlike other npm clients, Bun does not execute arbitrary lifecycle scripts like`postinstall` for installed dependencies. Executing arbitrary scripts represents a potential security risk.
To tell Bun to allow lifecycle scripts for a particular package, add the package to `trustedDependencies` in your package.json.
package.json

`my-trusted-package`.
Lifecycle scripts run in parallel during installation. To adjust the maximum number of concurrent scripts, use the `--concurrent-scripts` flag. The default is two times the reported cpu count or GOMAXPROCS.
terminal

`esbuild` and `sharp`) by determining which scripts need to run. To disable these optimizations:
terminal

## Workspaces

Bun supports`"workspaces"` in package.json. See [workspaces](/docs/pm/workspaces).

package.json

## Installing dependencies for specific packages

In a monorepo, you can install the dependencies for a subset of packages using the`--filter` flag.
terminal

[filtering](/docs/pm/filter#bun-install-and-bun-outdated).

## Overrides and resolutions

Bun supports npm’s`"overrides"` and Yarn’s `"resolutions"` in `package.json`. Both specify a version range for *metadependencies*, the dependencies of your dependencies. See

[overrides and resolutions](/docs/pm/overrides).

package.json

## Global packages

To install a package globally, use the`-g`/`--global` flag. Use it to install command-line tools.
terminal

## Production mode

To install in production mode (without`devDependencies`):
terminal

`--frozen-lockfile`. Bun installs the exact versions specified in the lockfile and does not update it. If your `package.json` disagrees with `bun.lock`, Bun exits with an error.
terminal

[lockfile](/docs/pm/lockfile)for more on

`bun.lock`.
## Omitting dependencies

To omit dev, peer, or optional dependencies, use the`--omit` flag.
terminal

## Dry run

To perform a dry run, without installing anything:terminal

## Non-npm dependencies

Bun supports installing dependencies from Git, GitHub, and local or remotely-hosted tarballs. See[.](/docs/pm/cli/add)

`bun add`package.json

## Installation strategies

Bun supports two package installation strategies that determine how dependencies are organized in`node_modules`:
### Hoisted installs

The traditional npm/Yarn approach that flattens dependencies into a shared`node_modules` directory:
terminal

### Isolated installs

A pnpm-like approach that creates strict dependency isolation to prevent[phantom dependencies](/docs/pm/isolated-installs), packages that can be imported without being declared in

`package.json`:
terminal

`node_modules/.bun/` with symlinks in the top-level `node_modules`. This ensures packages can only access their declared dependencies.
### Default strategy

The default linker strategy depends on whether you’re starting fresh or have an existing project:- **New workspaces/monorepos**:- `isolated`(prevents phantom dependencies)
- **New single-package projects**:- `hoisted`(traditional npm behavior)
- **Existing projects (made pre-v1.3.2)**:- `hoisted`(preserves backward compatibility)

`configVersion` field in your lockfile. For a detailed explanation, see [isolated installs](/docs/pm/isolated-installs).

## Minimum release age

To protect against supply chain attacks where malicious packages are quickly published, you can configure a minimum age requirement for npm packages. Bun filters out package versions published more recently than the specified threshold (in seconds) during installation.terminal

`bunfig.toml`:
bunfig.toml

- It only affects new package resolution; existing packages in `bun.lock`remain unchanged
- All dependencies (direct and transitive) are filtered to meet the age requirement when resolved
- When versions are blocked by the age gate, a stability check detects rapid bugfix patterns
- If multiple versions were published close together just outside your age gate, Bun extends the filter to skip those potentially unstable versions and selects an older, more mature version
- The check searches up to 7 days past the age gate; if releases are still rapid beyond that, Bun ignores the stability check
- Exact version requests (like `package@1.1.1`) still respect the age gate but bypass the stability check
 
- Versions without a `time`field are treated as passing the age check (the npm registry should always provide timestamps)

[Security Scanner API](/docs/pm/security-scanner-api).

## Configuration

### Configuring `bun install` with `bunfig.toml`

On `bun install`, `bun remove`, and `bun add`, Bun looks for `bunfig.toml` in:
- `$XDG_CONFIG_HOME/.bunfig.toml`or- `$HOME/.bunfig.toml`
- `./bunfig.toml`

`bunfig.toml` is optional. These are the default values:
bunfig.toml

### Configuring with environment variables

Environment variables take priority over`bunfig.toml`.
Bun uses the fastest installation method available on the target platform: 

`clonefile` on macOS and `hardlink` on Linux. You can change the installation method with the `--backend` flag. When unavailable or on error, `clonefile` and `hardlink` fall back to a platform-specific implementation of copying files.
Bun stores installed packages from npm in `~/.bun/install/cache/${name}@${version}`. If the semver version has a `build` or a `pre` tag, Bun replaces it with a hash of that value. This reduces the chances of errors from long file paths, but complicates figuring out where a package was installed on disk.
When the `node_modules` folder exists, Bun decides whether to install a package by checking that the `"name"` and `"version"` in its `package.json` at the expected `node_modules` location match the expected name and version. It uses a custom JSON parser which stops parsing as soon as it finds `"name"` and `"version"`.
When a `bun.lock` doesn’t exist or `package.json` has changed dependencies, Bun downloads and extracts tarballs eagerly while resolving.
When a `bun.lock` exists and `package.json` hasn’t changed, Bun downloads missing dependencies lazily. If the package with a matching `name` and `version` already exists in the expected location within `node_modules`, Bun won’t attempt to download the tarball.
## CI/CD

Use the official[action to install](https://github.com/oven-sh/setup-bun)

`oven-sh/setup-bun``bun` in a GitHub Actions pipeline:
.github/workflows/release.yml

`bun ci` to fail the build if the package.json is out of sync with the lockfile:
terminal

`bun ci` is equivalent to `bun install --frozen-lockfile`. It installs exact versions from `bun.lock` and fails if `package.json` doesn’t match the lockfile. To use `bun ci` or `bun install --frozen-lockfile`, you must commit `bun.lock` to version control.
In your workflow, run `bun ci` instead of `bun install`:
.github/workflows/release.yml

## Platform-specific dependencies?

Bun stores normalized`cpu` and `os` values from npm in the lockfile, along with the resolved packages. It skips downloading, extracting, and installing packages disabled for the current target at runtime. This means the lockfile won’t change between platforms/architectures even if the packages ultimately installed do change.
`--cpu` and `--os` flags

You can override the target platform for package selection:
**Accepted values for**:

`--cpu``arm`, `arm64`, `ia32`, `mips`, `mipsel`, `ppc`, `ppc64`, `s390`, `s390x`, `x32`, `x64`
**Accepted values for**:

`--os``aix`, `darwin`, `freebsd`, `linux`, `openbsd`, `sunos`, `win32`, `android`
## Peer dependencies?

Bun handles peer dependencies like Yarn:`bun install` installs them automatically. If the dependency is marked optional in `peerDependenciesMeta`, Bun uses an existing dependency if possible.
## Lockfile

`bun.lock` is Bun’s lockfile format. See [our blog post about the text lockfile](https://bun.com/blog/bun-lock-text-lockfile). Prior to Bun 1.2, the lockfile was binary and called

`bun.lockb`. To upgrade an old lockfile to the new format, run `bun install --save-text-lockfile --frozen-lockfile --lockfile-only`, then delete `bun.lockb`.
## Cache

To delete the cache:## Platform-specific backends

For performance,`bun install` uses different system calls to install dependencies depending on the platform. You can force a specific backend with the `--backend` flag.
**is the default backend on Linux. Benchmarking showed it to be the fastest on Linux.**

`hardlink`**is the default backend on macOS. Benchmarking showed it to be the fastest on macOS. It is only available on macOS.**

`clonefile`**is similar to**

`clonefile_each_dir``clonefile`, except it clones each file individually per directory. It is only available on macOS and tends to perform slower than `clonefile`. Unlike `clonefile`, this does not recursively clone subdirectories in one system call.
**is the fallback used when any of the above fail, and is the slowest. On macOS, it uses**

`copyfile``fcopyfile()`; on Linux, it uses `copy_file_range()`.
**is typically only used for**

`symlink``file:` dependencies (and eventually `link:`) internally. To prevent infinite loops, it skips symlinking the `node_modules` folder.
If you install with `--backend=symlink`, Node.js won’t resolve node_modules of dependencies unless each dependency has its own node_modules folder or you pass `--preserve-symlinks` to `node` or `bun`. See [Node.js documentation on](https://nodejs.org/api/cli.html#--preserve-symlinks).

`--preserve-symlinks`## npm registry metadata

Bun uses a binary format for caching npm registry responses. This loads much faster than JSON and tends to be smaller on disk. These files live in`~/.bun/install/cache/*.npm`. The filename pattern is `${hash(packageName)}.npm`. It’s a hash so that extra directories don’t need to be created for scoped packages.
Bun’s usage of `Cache-Control` ignores `Age`. This improves performance, but means Bun may be about 5 minutes behind the latest package version metadata from npm.
## pnpm migration

Bun migrates projects from pnpm automatically. When a`pnpm-lock.yaml` file is detected and no `bun.lock` file exists, Bun converts the lockfile to `bun.lock` during installation. The original `pnpm-lock.yaml` file remains unmodified.
terminal

`bun.lock` is absent. There is currently no opt-out flag for pnpm migration.
The migration process handles:
### Lockfile Migration

- Converts `pnpm-lock.yaml`to`bun.lock`format
- Preserves package versions and resolution information
- Maintains dependency relationships and peer dependencies
- Handles patched dependencies with integrity hashes

### Workspace Configuration

When a`pnpm-workspace.yaml` file exists, Bun migrates workspace settings to your root `package.json`:
pnpm-workspace.yaml

`workspaces` field in `package.json`:
package.json

### Catalog Dependencies

Dependencies using pnpm’s`catalog:` protocol are preserved:
package.json

### Configuration Migration

Bun migrates the following pnpm configuration from both`pnpm-lock.yaml` and `pnpm-workspace.yaml`:
- **Overrides**: Moved from- `pnpm.overrides`to root-level- `overrides`in- `package.json`
- **Patched Dependencies**: Moved from- `pnpm.patchedDependencies`to root-level- `patchedDependencies`in- `package.json`
- **Workspace Overrides**: Applied from- `pnpm-workspace.yaml`to root- `package.json`

### Requirements

- Requires pnpm lockfile version 7 or higher
- Workspace packages must have a `name`field in their`package.json`
- All catalog entries referenced by dependencies must exist in the catalogs definition

`pnpm-lock.yaml` and `pnpm-workspace.yaml` files.
## CLI Usage

terminal

### General Configuration

Specify path to config file (bunfig.toml)

Set a specific cwd

### Dependency Scope & Management

Don’t install devDependencies

Don’t update package.json or save a lockfile

Save to package.json

Exclude ‘dev’, ‘optional’, or ‘peer’ dependencies from install

Only add dependencies to package.json if they are not already present

### Dependency Type & Versioning

Add dependency to “devDependencies”

Add dependency to “optionalDependencies”

Add dependency to “peerDependencies”

Add the exact version instead of the ^ range

### Lockfile Control

Write a yarn.lock file (yarn v1)

Disallow changes to lockfile

Save a text-based lockfile

Generate a lockfile without installing dependencies

### Network & Registry Settings

Provide a Certificate Authority signing certificate

File path to Certificate Authority signing certificate

Use a specific registry by default, overriding .npmrc, bunfig.toml and environment variables

### Installation Process Control

Don’t install anything

Always request the latest versions from the registry & reinstall all dependencies

Install globally

Platform-specific optimizations: “clonefile”, “hardlink”, “symlink”, “copyfile”

Install packages for the matching workspaces

Recursively analyze & install all dependencies of files passed as arguments

### Caching Options

Store & load cached data from a specific directory path

Ignore manifest cache entirely

### Output & Logging

Don’t log anything

Excessively verbose logging

Disable the progress bar

Don’t print a summary

### Security & Integrity

Skip verifying integrity of newly downloaded packages

Add to trustedDependencies in the project’s package.json and install the package(s)

### Concurrency & Performance

Maximum number of concurrent jobs for lifecycle scripts (default: 2x CPU cores)

Maximum number of concurrent network requests

### Lifecycle Script Management

Skip lifecycle scripts in the project’s package.json (dependency scripts are never run)

### Help Information

Print this help menu

# Citations

1. Source page: https://bun.sh/docs/pm/cli/install
