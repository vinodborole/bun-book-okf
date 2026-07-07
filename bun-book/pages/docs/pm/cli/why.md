---
type: Web Page
title: bun why - Bun
description: Explain why a package is installed
resource: https://bun.sh/docs/pm/cli/why
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`bun why` explains why a package is installed in your project by showing the dependency chain that led to it.
## Usage

terminal

## Arguments

- `<package>`: The name of the package to explain. Supports glob patterns like- `@org/*`or- `*-lodash`.

## Options

- `--top`: Show only the top-level dependencies instead of the complete dependency tree.
- `--depth <number>`: Maximum depth of the dependency tree to display.

## Examples

Check why a specific package is installed:terminal

terminal

terminal

terminal

## Understanding the Output

The output shows:- The package name and version being queried
- The dependency chain that led to its installation
- The type of dependency (dev, peer, optional, or production)
- The version requirement specified in each package’s dependencies

# Citations

1. Source page: https://bun.sh/docs/pm/cli/why
