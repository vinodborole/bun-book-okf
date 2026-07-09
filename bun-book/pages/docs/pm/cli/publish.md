---
type: Web Page
title: bun publish - Bun
description: Use bun publish to publish a package to the npm registry
resource: https://bun.sh/docs/pm/cli/publish
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`bun publish` packs your package into a tarball, strips catalog and workspace protocols from the `package.json` (resolving versions if necessary), and publishes to the registry specified in your configuration files. Both `bunfig.toml` and `.npmrc` files are supported.
terminal

`bun pm pack`, then `bun publish` with the path to the output tarball.
terminal

`bun publish` does not run lifecycle scripts (`prepublishOnly/prepack/prepare/postpack/publish/postpublish`) if a
tarball path is provided. Scripts run only when `bun publish` packs the package itself.`--access`

`--access` sets the access level of the package being published, either `public` or `restricted`. Unscoped packages are always public, and publishing an unscoped package with `--access restricted` is an error.
terminal

`--access` can also be set in the `publishConfig` field of your `package.json`.
package.json

`--tag`

Set the tag of the package version being published. By default, the tag is `latest`. The initial version of a package is always given the `latest` tag in addition to the specified tag.
terminal

`--tag` can also be set in the `publishConfig` field of your `package.json`.
package.json

`--dry-run`

`--dry-run` runs the publish process without publishing the package, so you can verify what would be published.
terminal

`--tolerate-republish`

Exit with code 0 instead of 1 if the package version already exists. Useful in CI/CD where jobs may be re-run.
terminal

`--gzip-level`

Set the gzip compression level used when packing the package, from `0` to `9` (default `9`). Only applies to `bun publish` without a tarball path argument.
`--auth-type`

If you have 2FA enabled for your npm account, `bun publish` prompts you for a one-time password, either through a browser or in the CLI. `--auth-type` tells the npm registry which method you prefer: `web` (the default) or `legacy`.
terminal

`--otp`

Provide a one-time password directly to the CLI. If the password is valid, `bun publish` skips the extra one-time password prompt before publishing:
terminal

`bun publish` respects the `NPM_CONFIG_TOKEN` environment variable, useful when publishing from GitHub Actions or
other automated workflows.## CLI Usage

terminal

### Publishing Options

Set the access level of the package being published, either 

`public` or `restricted`. Unscoped packages are always public; publishing an unscoped package with `--access restricted` is an error.terminal

`--access` can also be set in the `publishConfig` field of your `package.json`.package.json

Set the tag of the package version being published. By default, the tag is 

`latest`. The initial version of a package is always given the `latest` tag in addition to the specified tag.terminal

`--tag` can also be set in the `publishConfig` field of your `package.json`.package.json

Simulate the publish process without publishing the package, to verify its contents first.

Specify the level of gzip compression to use when packing the package. Only applies to 

`bun publish` without a tarball
path argument. Values range from `0` to `9` (default is `9`).If you have 2FA enabled for your npm account, 

`bun publish` prompts you for a one-time password, either through a browser or the CLI. `--auth-type` tells the npm registry which method you prefer: `web` (the default) or `legacy`.terminal

Provide a one-time password directly to the CLI. A valid password skips the extra one-time password prompt before publishing.

terminal

`bun publish` respects the `NPM_CONFIG_TOKEN` environment variable, so you can publish from GitHub Actions or other
automated workflows.### Registry Configuration

#### Custom Registry

Specify registry URL, overriding .npmrc and bunfig.toml

#### SSL Certificates

Provide Certificate Authority signing certificate

Path to Certificate Authority certificate file

### Publishing Options

#### Dependency Management

Don’t install devDependencies

Exclude dependency types: 

`dev`, `optional`, or `peer`Always request the latest versions from the registry & reinstall all dependencies

#### Script Control

Skip lifecycle scripts during packing and publishing

Add packages to trustedDependencies and run their scripts

**Lifecycle Scripts**— When you publish a pre-built tarball, Bun does not run lifecycle scripts such as

`prepublishOnly` and `prepack`; they only run when Bun packs the package itself.#### File Management

Don’t update package.json or lockfile

Disallow changes to lockfile

Generate yarn.lock file (yarn v1 compatible)

#### Performance

Platform optimizations: 

`clonefile` (default), `hardlink`, `symlink`, or `copyfile`Maximum concurrent network requests

Maximum concurrent lifecycle scripts (default: 2x CPU cores)

#### Output Control

Suppress all output

Show detailed logging

Hide progress bar

Don’t print publish summary

# Citations

1. Source page: https://bun.sh/docs/pm/cli/publish
