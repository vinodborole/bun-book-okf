---
type: Web Page
title: Workspaces - Bun
description: Develop complex monorepos with multiple independent packages
resource: https://bun.sh/docs/pm/workspaces
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

[in](https://docs.npmjs.com/cli/v9/using-npm/workspaces?v=true#description)

`workspaces``package.json`. With workspaces, you develop several independent packages in a single repository, a *monorepo*. A monorepo commonly has this structure:

File Tree

`"workspaces"` key in the root `package.json` lists the subdirectories to treat as workspaces. By convention, they live in a directory called `packages`.
package.json

**Glob support**— Bun supports full glob syntax in

`"workspaces"`, including negative patterns such as
`!**/excluded/**`. See [supported glob patterns](/docs/runtime/glob#supported-glob-patterns).

package.json

`package.json`. To reference another package in the monorepo, use a semver range or the workspace protocol (for example `workspace:*`) as the version in your `package.json`.
packages/pkg-a/package.json

`bun install` installs dependencies for all workspaces in the monorepo, de-duplicating packages if possible. To install dependencies for specific workspaces only, use the `--filter` flag.
`workspace:` versions with the package’s `package.json` version:
`package.json` version:
- **Code can be split into logical parts.**If one package relies on another, add it as a dependency in- `package.json`. If package- `b`depends on- `a`,- `bun install`installs your local- `packages/a`directory into- `node_modules`instead of downloading it from the npm registry.
- **Dependencies can be de-duplicated.**If- `a`and- `b`share a common dependency, it is- *hoisted*to the root- `node_modules`directory. This saves disk space and minimizes the “dependency hell” of multiple versions of a package installed at once.
- **Run scripts in multiple packages.**Use the- `--filter`flag- `package.json`scripts in several packages at once, or- `--workspaces`to run scripts across all workspaces.

## Share versions with Catalogs

When many packages need the same dependency versions, define those versions once in a catalog in the root`package.json` and reference them from your workspaces
with the `catalog:` protocol. Updating the catalog updates every package that
references it. See [Catalogs](/docs/pm/catalogs).

⚡️ 

**Speed**— Installs are fast, even for big monorepos. Bun installs the[Remix](https://github.com/remix-run/remix)monorepo in about`500ms` on Linux.- 28x faster than `npm install`
- 12x faster than `yarn install`(v1)
- 8x faster than `pnpm install`

# Citations

1. Source page: https://bun.sh/docs/pm/workspaces
