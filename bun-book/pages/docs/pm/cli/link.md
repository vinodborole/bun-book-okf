---
type: Web Page
title: bun link - Bun
description: Link local packages for development
resource: https://bun.sh/docs/pm/cli/link
timestamp: '2026-08-03T08:59:43.078871+00:00'
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

boolean

Install globally. Alias: 

`-g`
### Dependency Management

boolean

Don’t install devDependencies. Alias: 

`-p`
string

Exclude 

`dev`, `optional`, or `peer` dependencies from install
### Project Files & Lockfiles

boolean

Write a 

`yarn.lock` file (yarn v1). Alias: `-y`
boolean

Disallow changes to lockfile

boolean

Save a text-based lockfile

boolean

Generate a lockfile without installing dependencies

boolean

Don’t update 

`package.json` or save a lockfile
boolean

default:"true"

Save to 

`package.json`
boolean

Add to 

`trustedDependencies` in the project’s `package.json` and install the package(s)
### Installation Control

boolean

Always request the latest versions from the registry & reinstall all dependencies. Alias: 

`-f`
boolean

Skip verifying integrity of newly downloaded packages

string

default:"clonefile"

Platform-specific optimizations for installing dependencies. One of 

`clonefile`, `hardlink`,
`symlink`, or `copyfile`
string

Linker strategy (one of 

`isolated` or `hoisted`)
boolean

Don’t install anything

boolean

Skip lifecycle scripts in the project’s 

`package.json` (dependency scripts are never run)
### Network & Registry

string

Provide a Certificate Authority signing certificate

string

Same as 

`—ca`, but as a file path to the certificate
string

Use a specific registry by default, overriding 

`.npmrc`, `bunfig.toml`, and environment
variables
number

default:"48"

Maximum number of concurrent network requests

### Performance & Resource

number

Maximum number of concurrent jobs for lifecycle scripts (default: 2x CPU cores)

### Caching

string

Store & load cached data from a specific directory path

boolean

Ignore manifest cache entirely

### Output & Logging

boolean

Don’t log anything

boolean

Only show tarball name when packing

boolean

Excessively verbose logging

boolean

Disable the progress bar

boolean

Don’t print a summary

### Platform Targeting

string

Override CPU architecture for optional dependencies (e.g., 

`x64`, `arm64`, `*` for
all)
string

Override operating system for optional dependencies (e.g., 

`linux`, `darwin`, `*` for
all)
### Global Configuration & Context

string

Specify path to config file (

`bunfig.toml`). Alias: `-c`
string

Set a specific current working directory

### Help

boolean

Print this help menu. Alias: 

`-h`

# Citations

1. Source page: https://bun.sh/docs/pm/cli/link
