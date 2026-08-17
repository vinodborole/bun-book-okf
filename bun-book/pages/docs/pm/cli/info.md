---
type: Web Page
title: bun info | Bun Docs
description: Display package metadata from the npm registry
resource: https://bun.sh/docs/pm/cli/info
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# bun info

Display package metadata from the npm registry

`bun info` displays package metadata from the npm registry.

## Usage

terminal

`bun info react`
`bun info react` prints the package's latest version, description, homepage, dependencies, and other metadata.

## Viewing specific versions

To view information about a specific version:

terminal

`bun info react@18.0.0`
## Viewing specific properties

To print specific properties from the package metadata:

terminal

```
bun info react version
bun info react dependencies
bun info react repository.url
```
## JSON output

To get the output in JSON format, use the `--json` flag:

terminal

`bun info react --json`
## Alias

`bun pm view` is an alias for `bun info`:

terminal

`bun pm view react  # equivalent to: bun info react`
## Examples

terminal

```
# View basic package information
bun info is-number
# View a specific version
bun info is-number@7.0.0
# View all available versions
bun info is-number versions
# View package dependencies
bun info express dependencies
# View package homepage
bun info lodash homepage
# Get JSON output
bun info react --json
```

# Citations

1. Source page: https://bun.sh/docs/pm/cli/info
