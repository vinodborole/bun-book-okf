---
type: Web Page
title: Auto-install - Bun
description: Bun's automatic package installation feature for standalone script execution
resource: https://bun.sh/docs/runtime/auto-install
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`node_modules` directory in the working directory or higher, it abandons Node.js-style module resolution in favor of the **Bun module resolution algorithm**. Under Bun-style module resolution, Bun auto-installs every imported package on the fly into a

[global module cache](/docs/pm/global-cache)during execution (the same cache used by

[).](/docs/pm/cli/install)

`bun install`index.ts

`"foo"` and caches it. Later runs use the cached version.
## Version resolution

Bun determines which version to install as follows:- Check for a `bun.lock`file in the project root. If it exists, use the version specified in the lockfile.
- Otherwise, scan up the tree for a `package.json`that includes`"foo"`as a dependency. If found, use the specified semver version or version range.
- Otherwise, use `latest`.

## Cache behavior

Once Bun determines a version or version range, it:- Checks the module cache for a compatible version. If one exists, uses it.
- When resolving `latest`, checks if`package@latest`was downloaded and cached in the last*24 hours*. If so, uses it.
- Otherwise, downloads and installs the appropriate version from the `npm`registry.

## Installation

Bun installs and caches packages into`<cache>/<pkg>@<version>`, so multiple versions of the same package can be cached at once. It also creates a symlink under `<cache>/<pkg>/<version>` to speed up looking up all cached versions of a package.
## Version specifiers

To bypass version resolution entirely, specify a version or version range directly in your import statement.index.ts

## Benefits

- **Space efficiency**— Each version of a dependency exists in only one place on disk. This saves space and time compared to redundant per-project installations.
- **Portability**— Your source file is- *self-contained*, so sharing scripts and gists doesn’t mean zipping up a directory of code and config files. With version specifiers in- `import`statements, even a- `package.json`isn’t necessary.
- **Convenience**— You don’t need to run- `npm install`or- `bun install`before running a file or script with- `bun run`.
- **Backwards compatibility**— Because Bun still respects the versions specified in- `package.json`if one exists, you can switch to Bun-style resolution with a single command:- `rm -rf node_modules`.

## Limitations

- No Intellisense. TypeScript auto-completion in IDEs relies on type declaration files inside `node_modules`. We are investigating solutions to this.
- No [patch-package](https://github.com/ds300/patch-package)support

## FAQ

How is this different from what pnpm does?

How is this different from what pnpm does?

With pnpm, you have to run 

`pnpm install`, which creates a `node_modules` folder of symlinks for the runtime to resolve. By contrast, Bun resolves dependencies on the fly when you run a file; there’s no need to run any `install` command ahead of time. Bun also doesn’t create a `node_modules` folder.How is this different from Yarn Plug'N'Play does?

How is this different from Yarn Plug'N'Play does?

With Yarn, you must run 

`yarn install` before you run a script. By contrast, Bun resolves dependencies on the fly when you run a file; there’s no need to run any `install` command ahead of time.Yarn Plug’N’Play also uses zip files to store dependencies. This makes dependency loading [slower at runtime](https://twitter.com/jarredsumner/status/1458207919636287490), as random access reads on zip files tend to be slower than the equivalent disk lookup.How is this different from what Deno does?

How is this different from what Deno does?

Deno requires an 

`npm:` specifier before each npm `import`, lacks support for import maps through `compilerOptions.paths` in `tsconfig.json`, and has incomplete support for `package.json` settings. Unlike Deno, Bun does not currently support URL imports.

# Citations

1. Source page: https://bun.sh/docs/runtime/auto-install
