---
type: Web Page
title: bun add - Bun
description: Add packages to your project with Bun's fast package manager
resource: https://bun.sh/docs/pm/cli/add
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

terminal

terminal

`--dev`

**Alias**—

`--development`, `-d`, `-D``"devDependencies"`):
terminal

`--optional`

To add a package as an optional dependency (`"optionalDependencies"`):
terminal

`--peer`

To add a package as a peer dependency (`"peerDependencies"`):
terminal

`--exact`

**Alias**—

`-E``--exact`. Bun writes the exact version number to your `package.json` instead of a version range.
terminal

`package.json`:
package.json

terminal

`--global`

**Alias**—

`bun add --global`, `bun add -g`, `bun install --global` and `bun install -g``-g`/`--global` flag. This does not modify the `package.json` of your current project. Use it to install command-line tools.
terminal

Configuring global installation behavior

Configuring global installation behavior

bunfig.toml

## Trusted dependencies

Unlike other npm clients, Bun does not execute arbitrary lifecycle scripts for installed dependencies, such as`postinstall`. These scripts represent a potential security risk, as they can execute arbitrary code on your machine.
To tell Bun to allow lifecycle scripts for a particular package, add the package to `trustedDependencies` in your package.json.
package.json

`my-trusted-package`.
## Git dependencies

To add a dependency from a public or private git repository:terminal

To install private repositories, your system needs the appropriate SSH credentials to access the repository.

[,](https://docs.npmjs.com/cli/v9/configuring-npm/package-json#github-urls)

`github`[,](https://docs.npmjs.com/cli/v9/configuring-npm/package-json#git-urls-as-dependencies)

`git``git+ssh`, and `git+https`.
package.json

## Tarball dependencies

A package name can correspond to a publicly hosted`.tgz` file. Bun downloads and installs the package from that tarball URL rather than from the package registry.
terminal

`bun add` writes the URL to your `package.json`:
package.json

## CLI Usage

### Dependency Management

Don’t install devDependencies. Alias: 

`-p`Exclude 

`dev`, `optional`, or `peer` dependencies from installInstall globally. Alias: 

`-g`Add dependency to 

`devDependencies`. Alias: `-d`Add dependency to 

`optionalDependencies`Add dependency to 

`peerDependencies`Add the exact version instead of the 

`^` range. Alias: `-E`Only add dependencies to 

`package.json` if they are not already present### Project Files & Lockfiles

Write a 

`yarn.lock` file (yarn v1). Alias: `-y`Don’t update 

`package.json` or save a lockfileSave to 

`package.json`Disallow changes to lockfile

Add to 

`trustedDependencies` in the project’s `package.json` and install the package(s)Save a text-based lockfile

Generate a lockfile without installing dependencies

### Installation Control

Don’t install anything

Always request the latest versions from the registry & reinstall all dependencies. Alias: 

`-f`Skip verifying integrity of newly downloaded packages

Skip lifecycle scripts in the project’s 

`package.json` (dependency scripts are never run)Recursively analyze & install dependencies of files passed as arguments (using Bun’s bundler). Alias:

`-a`### Network & Registry

Provide a Certificate Authority signing certificate

Same as 

`—ca`, but as a file path to the certificateUse a specific registry by default, overriding 

`.npmrc`, `bunfig.toml`, and environment
variablesMaximum number of concurrent network requests

### Performance & Resource

Platform-specific optimizations for installing dependencies. One of 

`clonefile`, `hardlink`,
`symlink`, or `copyfile`Maximum number of concurrent jobs for lifecycle scripts (default: 2x CPU cores)

### Caching

Store & load cached data from a specific directory path

Ignore manifest cache entirely

### Output & Logging

Don’t log anything

Excessively verbose logging

Disable the progress bar

Don’t print a summary

### Global Configuration & Context

Specify path to config file (

`bunfig.toml`). Alias: `-c`Set a specific current working directory

### Help

Print this help menu. Alias: 

`-h`

# Citations

1. Source page: https://bun.sh/docs/pm/cli/add
