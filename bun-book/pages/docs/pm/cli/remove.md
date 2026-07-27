---
type: Web Page
title: bun remove - Bun
description: Remove dependencies from your project
resource: https://bun.sh/docs/pm/cli/remove
timestamp: '2026-07-27T09:26:27.222623+00:00'
---

## Basic Usage

terminal

## CLI Usage

terminal

### General Information

boolean

Print this help menu. Alias: 

`-h`### Configuration

string

Specify path to config file (

`bunfig.toml`). Alias: `-c`### Package.json Interaction

boolean

Don’t update 

`package.json` or save a lockfileboolean

default:"true"

Save to 

`package.json` (true by default)boolean

Add to 

`trustedDependencies` in the project’s `package.json` and install the package(s)### Lockfile Behavior

boolean

Write a 

`yarn.lock` file (yarn v1). Alias: `-y`boolean

Disallow changes to lockfile

boolean

Save a text-based lockfile

boolean

Generate a lockfile without installing dependencies

### Dependency Filtering

boolean

Don’t install devDependencies. Alias: 

`-p`string

Exclude 

`dev`, `optional`, or `peer` dependencies from install### Network & Registry

string

Provide a Certificate Authority signing certificate

string

Same as 

`—ca`, but as a file path to the certificatestring

Use a specific registry by default, overriding 

`.npmrc`, `bunfig.toml` and environment variables### Execution Control & Validation

boolean

Don’t install anything

boolean

Always request the latest versions from the registry & reinstall all dependencies. Alias: 

`-f`boolean

Skip verifying integrity of newly downloaded packages

### Output & Logging

boolean

Don’t log anything

boolean

Excessively verbose logging

boolean

Disable the progress bar

boolean

Don’t print a summary

### Caching

string

Store & load cached data from a specific directory path

boolean

Ignore manifest cache entirely

### Script Execution

boolean

Skip lifecycle scripts in the project’s 

`package.json` (dependency scripts are never run)number

Maximum number of concurrent jobs for lifecycle scripts (default: 2x CPU cores)

### Scope & Path

boolean

Install globally. Alias: 

`-g`string

Set a specific cwd

### Advanced & Performance

string

default:"clonefile"

Platform-specific optimizations for installing dependencies. Possible values: 

`clonefile` (default),
`hardlink`, `symlink`, `copyfile`number

default:"48"

Maximum number of concurrent network requests (default 48)

# Citations

1. Source page: https://bun.sh/docs/pm/cli/remove
