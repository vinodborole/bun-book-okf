---
type: Web Page
title: bun remove | Bun Docs
description: Remove dependencies from your project
resource: https://bun.sh/docs/pm/cli/remove
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# bun remove

Remove dependencies from your project

## Basic Usage

**Alias**—

`bun rm`, `bun uninstall`, `bun r``bun remove ts-node`
Bun removes the package from every dependency group in `package.json` that lists it and updates `bun.lock`. Once nothing else depends on the package, Bun deletes it from `node_modules`.

## `--filter`

**Alias**—

`-F`
In a monorepo, remove the package from the matching workspace(s) instead of the current directory's package, using the same patterns as [`bun add --filter`](/docs/pm/cli/add#--filter). Use `--filter '*'` to remove it from every workspace package. Workspaces that don't list the package are left untouched.

```
bun remove zod --filter api
bun remove zod --filter '*'
```
## CLI Usage

`bun remove <package>`
### General Information

Print this help menu. Alias: `-h`

### Configuration

Specify path to config file (`bunfig.toml`). Alias: `-c`

### Package.json Interaction

Don't update `package.json` or save a lockfile

Save to `package.json` (true by default)

Add to `trustedDependencies` in the project's `package.json` and install the package(s)

Remove the package(s) from the matching workspaces instead of the current package. Alias: `-F`

### Lockfile Behavior

Write a `yarn.lock` file (yarn v1). Alias: `-y`

Disallow changes to lockfile

Save a text-based lockfile

Generate a lockfile without installing dependencies

### Dependency Filtering

Don't install devDependencies. Alias: `-p`

Exclude `dev`, `optional`, or `peer` dependencies from install

### Network & Registry

Provide a Certificate Authority signing certificate

Same as `--ca`, but as a file path to the certificate

Use a specific registry by default, overriding `.npmrc`, `bunfig.toml` and environment variables

### Execution Control & Validation

Resolve the change but don't remove packages, update `package.json`, or save a lockfile (the project's own
lifecycle scripts still run)

Always request the latest versions from the registry & reinstall all dependencies. Alias: `-f`

Skip verifying integrity of newly downloaded packages

### Output & Logging

Don't log anything

Excessively verbose logging

Disable the progress bar

Don't print a summary

### Caching

Store & load cached data from a specific directory path

Ignore manifest cache entirely

### Script Execution

Skip lifecycle scripts for all packages, including the project's `package.json` and trusted dependencies

Maximum number of concurrent jobs for lifecycle scripts (default: 2x CPU cores)

### Scope & Path

Install globally. Alias: `-g`

Set a specific cwd

### Advanced & Performance

Platform-specific optimizations for installing dependencies. Possible values: `clonefile` (default on
macOS), `hardlink` (default on Linux and Windows), `symlink`, `copyfile`

Maximum number of concurrent network requests (default 48)

# Citations

1. Source page: https://bun.sh/docs/pm/cli/remove
