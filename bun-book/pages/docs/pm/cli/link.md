---
type: Web Page
title: bun link - Bun
description: Link local packages for development
resource: https://bun.sh/docs/pm/cli/link
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`bun link` in a local directory to register the current package as a “linkable” package.
terminal

`bun link cool-pkg`, which creates a symlink in the target project’s `node_modules` directory pointing to the local directory.
terminal

`--save` flag also adds `cool-pkg` to the `dependencies` field of your app’s package.json, with a version specifier that tells Bun to load from the registered local directory instead of installing from `npm`:
package.json

## Unlinking

Use`bun unlink` in the root directory to unregister a local package.
terminal

# CLI Usage

### Installation Scope

Install globally. Alias: 

`-g`### Dependency Management

Don’t install devDependencies. Alias: 

`-p`Exclude 

`dev`, `optional`, or `peer` dependencies from install### Project Files & Lockfiles

Write a 

`yarn.lock` file (yarn v1). Alias: `-y`Disallow changes to lockfile

Save a text-based lockfile

Generate a lockfile without installing dependencies

Don’t update 

`package.json` or save a lockfileSave to 

`package.json`Add to 

`trustedDependencies` in the project’s `package.json` and install the package(s)### Installation Control

Always request the latest versions from the registry & reinstall all dependencies. Alias: 

`-f`Skip verifying integrity of newly downloaded packages

Platform-specific optimizations for installing dependencies. One of 

`clonefile`, `hardlink`,
`symlink`, or `copyfile`Linker strategy (one of 

`isolated` or `hoisted`)Don’t install anything

Skip lifecycle scripts in the project’s 

`package.json` (dependency scripts are never run)### Network & Registry

Provide a Certificate Authority signing certificate

Same as 

`—ca`, but as a file path to the certificateUse a specific registry by default, overriding 

`.npmrc`, `bunfig.toml`, and environment
variablesMaximum number of concurrent network requests

### Performance & Resource

Maximum number of concurrent jobs for lifecycle scripts (default: 2x CPU cores)

### Caching

Store & load cached data from a specific directory path

Ignore manifest cache entirely

### Output & Logging

Don’t log anything

Only show tarball name when packing

Excessively verbose logging

Disable the progress bar

Don’t print a summary

### Platform Targeting

Override CPU architecture for optional dependencies (e.g., 

`x64`, `arm64`, `*` for
all)Override operating system for optional dependencies (e.g., 

`linux`, `darwin`, `*` for
all)### Global Configuration & Context

Specify path to config file (

`bunfig.toml`). Alias: `-c`Set a specific current working directory

### Help

Print this help menu. Alias: 

`-h`

# Citations

1. Source page: https://bun.sh/docs/pm/cli/link
