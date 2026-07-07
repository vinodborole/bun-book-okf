---
type: Web Page
title: bun outdated - Bun
description: Check for outdated dependencies
resource: https://bun.sh/docs/pm/cli/outdated
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`bun outdated` displays a table of the dependencies in your project that have newer versions available.
terminal

## Version Information

The output table shows three version columns:- **Current**: The version currently installed
- **Update**: The latest version that satisfies your package.json version range
- **Latest**: The latest version published to the registry

### Dependency Filters

To check specific dependencies, pass their names as positional arguments:terminal

terminal

`@types/*` packages:
terminal

`@types/*` packages:
terminal

### Workspace Filters

Use the`--filter` flag to check for outdated dependencies in a different workspace package:
terminal

`--filter` more than once to check multiple workspaces:
terminal

`--filter` also accepts glob patterns for workspace names:
terminal

### Catalog Dependencies

`bun outdated` also checks catalog dependencies defined in `package.json`:
terminal

## CLI Usage

terminal

### General Options

Specify path to config file (

`bunfig.toml`)Set a specific cwd

Print this help menu

Display outdated dependencies for each matching workspace

### Output & Logging

Don’t log anything

Excessively verbose logging

Disable the progress bar

Don’t print a summary

### Dependency Scope & Target

Don’t install devDependencies

Exclude 

`dev`, `optional`, or `peer` dependencies from installInstall globally

### Lockfile & Package.json

Write a 

`yarn.lock` file (yarn v1)Don’t update 

`package.json` or save a lockfileSave to 

`package.json` (true by default)Disallow changes to lockfile

Save a text-based lockfile

Generate a lockfile without installing dependencies

Add to 

`trustedDependencies` in the project’s `package.json` and install the package(s)### Network & Registry

Provide a Certificate Authority signing certificate

Same as 

`—ca`, but as a file path to the certificateUse a specific registry by default, overriding 

`.npmrc`, `bunfig.toml` and environment variablesMaximum number of concurrent network requests (default 48)

### Caching

Store & load cached data from a specific directory path

Ignore manifest cache entirely

### Execution Behavior

Don’t install anything

Always request the latest versions from the registry & reinstall all dependencies

Skip verifying integrity of newly downloaded packages

Skip lifecycle scripts in the project’s 

`package.json` (dependency scripts are never run)Platform-specific optimizations for installing dependencies. Possible values: 

`clonefile` (default),
`hardlink`, `symlink`, `copyfile`Maximum number of concurrent jobs for lifecycle scripts (default 5)

# Citations

1. Source page: https://bun.sh/docs/pm/cli/outdated
