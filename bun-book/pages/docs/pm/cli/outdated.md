---
type: Web Page
title: bun outdated - Bun
description: Check for outdated dependencies
resource: https://bun.sh/docs/pm/cli/outdated
timestamp: '2026-07-27T09:26:27.222623+00:00'
---

`bun outdated` displays a table of the dependencies in your project that have newer versions available.
terminal

## Version Information

The output table shows three version columns:- **Current**: The version currently installed
- **Update**: The latest version that satisfies your package.json version range
- **Latest**: The latest version published to the registry

### Dependency Filters

To check a specific dependency, pass its name as a positional argument:terminal

terminal

`@types/*` packages:
terminal

`@types/*` packages:
terminal

### Workspace Filters

Use the`--filter` flag to check for outdated dependencies in a different workspace package:
terminal

`--filter` accepts glob patterns to match multiple workspaces:
terminal

### Catalog Dependencies

`bun outdated` also checks [catalog](/docs/pm/catalogs)dependencies defined in

`package.json`:
terminal

## CLI Usage

terminal

### General Options

string

Specify path to config file (

`bunfig.toml`)string

Set a specific cwd

boolean

Print this help menu

string

Display outdated dependencies for each matching workspace

### Output & Logging

boolean

Don’t log anything

boolean

Excessively verbose logging

boolean

Disable the progress bar

boolean

Don’t print a summary

### Dependency Scope & Target

boolean

Don’t install devDependencies

string

Exclude 

`dev`, `optional`, or `peer` dependencies from installboolean

Install globally

### Lockfile & Package.json

boolean

Write a 

`yarn.lock` file (yarn v1)boolean

Don’t update 

`package.json` or save a lockfileboolean

default:"true"

Save to 

`package.json` (true by default)boolean

Disallow changes to lockfile

boolean

Save a text-based lockfile

boolean

Generate a lockfile without installing dependencies

boolean

Add to 

`trustedDependencies` in the project’s `package.json` and install the package(s)### Network & Registry

string

Provide a Certificate Authority signing certificate

string

Same as 

`—ca`, but as a file path to the certificatestring

Use a specific registry by default, overriding 

`.npmrc`, `bunfig.toml` and environment variablesnumber

default:"48"

Maximum number of concurrent network requests (default 48)

### Caching

string

Store & load cached data from a specific directory path

boolean

Ignore manifest cache entirely

### Execution Behavior

boolean

Don’t install anything

boolean

Always request the latest versions from the registry & reinstall all dependencies

boolean

Skip verifying integrity of newly downloaded packages

boolean

Skip lifecycle scripts in the project’s 

`package.json` (dependency scripts are never run)string

default:"clonefile"

Platform-specific optimizations for installing dependencies. Possible values: 

`clonefile` (default),
`hardlink`, `symlink`, `copyfile`number

Maximum number of concurrent jobs for lifecycle scripts (default: 2x CPU cores)

# Citations

1. Source page: https://bun.sh/docs/pm/cli/outdated
