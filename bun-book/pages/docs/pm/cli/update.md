---
type: Web Page
title: bun update - Bun
description: Update dependencies to latest versions
resource: https://bun.sh/docs/pm/cli/update
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

To upgrade your Bun CLI version, see 

[.](/docs/installation#upgrading)`bun upgrade`terminal

terminal

`--interactive`

Use the `--interactive` flag to choose which packages to update:
terminal

### Interactive Interface

The interface displays packages grouped by dependency type:**Sections:**

- Packages are grouped under section headers: `dependencies`,`devDependencies`,`peerDependencies`,`optionalDependencies`
- Each section shows column headers aligned with the package data

**Columns:**

- **Package**: Package name (may have a suffix such as- `dev`,- `peer`, or- `optional`)
- **Current**: Currently installed version
- **Target**: Version that would be installed (respects semver constraints)
- **Latest**: Latest available version

### Keyboard Controls

**Selection:**

- **Space**: Toggle package selection
- **Enter**: Confirm selections and update
- **a/A**: Select all packages
- **n/N**: Select none
- **i/I**: Invert selection

**Navigation:**

- **↑/↓ Arrow keys**or- **j/k**: Move cursor
- **l/L**: Toggle between target and latest version for current package

**Exit:**

- **Ctrl+C**or- **Ctrl+D**: Cancel without updating

### Visual Indicators

- **■**Selected packages (will be updated)
- **□**Unselected packages
- **❯**Current cursor position
- **Colors**: Red (major), yellow (minor), green (patch) version changes
- **Underlined**: Currently selected update target

### Package Grouping

Packages are organized in sections by dependency type:- **dependencies**- Regular runtime dependencies
- **devDependencies**- Development dependencies
- **peerDependencies**- Peer dependencies
- **optionalDependencies**- Optional dependencies

` dev`, ` peer`, ` optional`).
`--recursive`

Use the `--recursive` flag with `--interactive` to update dependencies across all workspaces in a monorepo:
terminal

`--recursive`, the interface adds a “Workspace” column showing which workspace each dependency belongs to.
`--latest`

By default, `bun update` updates each dependency to the latest version that satisfies the version range in your `package.json`.
To update to the latest version regardless of whether it satisfies that range, use the `--latest` flag:
terminal

**l**to toggle a package between its target version (respecting semver) and the latest version. For example, with the following

`package.json`:
package.json

- `bun update`would update to a version that matches- `17.x`.
- `bun update --latest`would update to a version that matches- `18.x`or later.

## CLI Usage

terminal

### Update Strategy

Always request the latest versions from the registry & reinstall all dependencies. Alias: 

`-f`Update packages to their latest versions

### Dependency Scope

Don’t install devDependencies. Alias: 

`-p`Install globally. Alias: 

`-g`Exclude 

`dev`, `optional`, or `peer` dependencies from install### Project File Management

Write a 

`yarn.lock` file (yarn v1). Alias: `-y`Don’t update 

`package.json` or save a lockfileSave to 

`package.json` (true by default)Disallow changes to lockfile

Save a text-based lockfile

Generate a lockfile without installing dependencies

### Network & Registry

Provide a Certificate Authority signing certificate

Same as 

`—ca`, but as a file path to the certificateUse a specific registry by default, overriding 

`.npmrc`, `bunfig.toml` and environment variablesMaximum number of concurrent network requests (default 48)

### Caching

Store & load cached data from a specific directory path

Ignore manifest cache entirely

### Output & Logging

Don’t log anything

Excessively verbose logging

Disable the progress bar

Don’t print a summary

### Script Execution

Skip lifecycle scripts in the project’s 

`package.json` (dependency scripts are never run)Maximum number of concurrent jobs for lifecycle scripts (default: 2x CPU cores)

### Installation Controls

Skip verifying integrity of newly downloaded packages

Add to 

`trustedDependencies` in the project’s `package.json` and install the package(s)Platform-specific optimizations for installing dependencies. Possible values: 

`clonefile` (default),
`hardlink`, `symlink`, `copyfile`### General & Environment

Specify path to config file (

`bunfig.toml`). Alias: `-c`Don’t install anything

Set a specific cwd

Print this help menu. Alias: 

`-h`

# Citations

1. Source page: https://bun.sh/docs/pm/cli/update
