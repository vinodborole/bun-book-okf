---
type: Web Page
title: Catalogs - Bun
description: Share common dependency versions across multiple packages in a monorepo
resource: https://bun.sh/docs/pm/catalogs
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

**Catalogs**share dependency versions across the packages in a monorepo. Rather than repeating the same versions in each workspace package, you define them once in the root

`package.json` and reference them throughout your project.
## Overview

Instead of each workspace package specifying its own versions, you:- Define version catalogs in the root `package.json`
- Reference those versions with the `catalog:`protocol
- Update every package at once by changing the version in one place

## How to Use Catalogs

### Directory Structure Example

Consider a monorepo with the following structure:### 1. Define Catalogs in Root package.json

In your root-level`package.json`, add a `catalog` or `catalogs` field within the `workspaces` object:
package.json

`catalog` and `catalogs` also work at the top level of `package.json`.
### 2. Reference Catalog Versions in Workspace Packages

In your workspace packages, use the`catalog:` protocol to reference versions:
packages/app/package.json

packages/ui/package.json

### 3. Run Bun Install

Run`bun install` to install all dependencies according to the catalog versions.
## Catalog vs Catalogs

Bun supports two ways to define catalogs:- 
`catalog`Reference withpackage.json`catalog:`:packages/app/package.json
- 
`catalogs`Reference withpackage.json`catalog:<name>`:packages/app/package.json

## Benefits of Using Catalogs

- **Consistency**: All packages use the same version of critical dependencies
- **Maintenance**: Update a dependency version in one place instead of across multiple- `package.json`files
- **Clarity**: Makes it obvious which dependencies are standardized across your monorepo
- **Simplicity**: No extra version resolution strategies or external tools

## Real-World Example

A larger example, for a React application:**Root package.json**

package.json

packages/app/package.json

packages/ui/package.json

packages/utils/package.json

## Updating Versions

To update versions across all packages, change the version in the root package.json:package.json

`bun install` to update all packages.
## Lockfile Integration

Bun’s lockfile tracks catalog versions, so installs are consistent across environments. The lockfile includes:- The catalog definitions from your package.json
- The resolution of each cataloged dependency

bun.lock(excerpt)

## Limitations and Edge Cases

- Catalog references must match a dependency defined in either `catalog`or one of the named`catalogs`
- Empty strings and whitespace in catalog names are ignored (treated as default catalog)
- Invalid dependency versions in catalogs fail to resolve during `bun install`
- Catalogs are only available within workspaces; they cannot be used outside the monorepo

## Publishing

When you run`bun publish` or `bun pm pack`, Bun replaces `catalog:` references
in your `package.json` with the resolved version numbers. The published package
includes regular semver strings and no longer depends on your catalog
definitions.

# Citations

1. Source page: https://bun.sh/docs/pm/catalogs
