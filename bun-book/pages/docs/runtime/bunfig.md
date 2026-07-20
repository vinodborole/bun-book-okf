---
type: Web Page
title: bunfig.toml - Bun
description: Configure Bun's behavior using its configuration file bunfig.toml
resource: https://bun.sh/docs/runtime/bunfig
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

`bunfig.toml` is Bun’s configuration file.
Bun relies on pre-existing configuration files like `package.json` and `tsconfig.json` where it can; `bunfig.toml` is only for Bun-specific settings. The file is optional, and Bun works without it.
## Global vs. local

Put`bunfig.toml` in your project root, alongside your `package.json`.
To configure Bun globally, you can also create a `.bunfig.toml` file at one of the following paths:
- `$HOME/.bunfig.toml`
- `$XDG_CONFIG_HOME/.bunfig.toml`

`bunfig`, it shallow-merges them, with local overriding global. CLI flags override `bunfig` settings where applicable.
## Runtime

Top-level fields in`bunfig.toml` configure Bun’s runtime behavior.
`preload`

An array of scripts/plugins to execute before running a file or script.
bunfig.toml

`jsx`

Configure how Bun handles JSX. You can also set these fields in the `compilerOptions` of your `tsconfig.json`, but `bunfig.toml` supports them for non-TypeScript projects.
bunfig.toml

`smol`

Enable `smol` mode. This reduces memory usage at the cost of performance.
bunfig.toml

`logLevel`

Set the log level: `"debug"`, `"warn"`, or `"error"`.
bunfig.toml

`define`

The `define` field replaces global identifiers with constant expressions wherever they appear. The expression should be a JSON string.
bunfig.toml

`loader`

Configure how Bun maps file extensions to [loaders](/docs/bundler/loaders). Use this to load file types Bun doesn’t support natively.

bunfig.toml

- `jsx`
- `js`
- `ts`
- `tsx`
- `css`
- `file`
- `json`
- `toml`
- `wasm`
- `napi`
- `base64`
- `dataurl`
- `text`

`telemetry`

The `telemetry` field enables or disables analytics. By default, telemetry is enabled. This is equivalent to the `DO_NOT_TRACK` environment variable.
We do not currently collect telemetry; this setting only controls anonymous crash reports. We plan to collect information like which Bun APIs are used most or how long `bun build` takes.
bunfig.toml

`env`

Configure automatic `.env` file loading. Bun loads `.env` files by default. To disable this behavior:
bunfig.toml

`file` property:
bunfig.toml

`--env-file` are still loaded even when default loading is disabled.
`console`

Configure console output behavior.
`console.depth`

Set the default depth for `console.log()` object inspection. Default `2`.
bunfig.toml

`--console-depth` CLI flag overrides this setting.
## Serve

The`[serve]` section configures `Bun.serve` and `bun run` when serving HTTP.
`serve.port`

The default port for `Bun.serve` to listen on. Default `3000`. Can also be set with the `BUN_PORT` or `PORT` environment variables, or the `--port` flag.
bunfig.toml

## Test runner

The`[test]` section of `bunfig.toml` configures the test runner.
bunfig.toml

`test.root`

The root directory to run tests from. Default `.`.
bunfig.toml

`test.preload`

Same as the top-level `preload` field, but only applies to `bun test`.
bunfig.toml

`test.pathIgnorePatterns`

Exclude files and directories from test discovery using glob patterns. Matched directories are pruned during scanning, so their contents are never traversed. Use this when your project contains submodules or vendored code with `*.test.ts` files that you don’t want `bun test` to pick up.
bunfig.toml

`--path-ignore-patterns`. CLI flags override the `bunfig.toml` value entirely.
`test.smol`

Same as the top-level `smol` field, but only applies to `bun test`.
bunfig.toml

`test.coverage`

Enables coverage reporting. Default `false`. Use `--coverage` to override.
bunfig.toml

`test.coverageThreshold`

The coverage threshold. By default, no threshold is set. If your test suite does not meet it, `bun test` exits with a non-zero exit code.
bunfig.toml

bunfig.toml

`test.coverageSkipTestFiles`

Whether to skip test files when computing coverage statistics. Default `false`.
bunfig.toml

`test.coverageIgnoreSourcemaps`

Whether to report coverage against transpiled output instead of remapping line numbers through sourcemaps back to the original source. Default `false`. Primarily useful for debugging.
bunfig.toml

`test.coveragePathIgnorePatterns`

Exclude files from coverage reports using glob patterns. Accepts a single pattern or an array of patterns.
bunfig.toml

`test.coverageReporter`

By default, coverage reports are printed to the console. For persistent reports that CI and other tools can read, use `lcov`.
bunfig.toml

`test.coverageDir`

Set the path where coverage reports are saved. This only applies to persistent reporters like `lcov`.
bunfig.toml

`test.randomize`

Run tests in random order. Default `false`.
bunfig.toml

`seed`, the random order becomes reproducible.
The `--randomize` CLI flag overrides this setting.
`test.seed`

Set the random seed for test randomization. This option requires `randomize` to be `true`.
bunfig.toml

`--seed` CLI flag overrides this setting.
`test.rerunEach`

Re-run each test file a specified number of times. Default `0` (run once).
bunfig.toml

`--rerun-each` CLI flag overrides this setting.
`test.retry`

Default retry count for all tests. Failed tests are retried up to this many times. Per-test `{ retry: N }` overrides this value. Default `0` (no retries).
bunfig.toml

`--retry` CLI flag overrides this setting.
`test.concurrentTestGlob`

Test files matching this glob pattern run all of their tests concurrently, as if you passed the `--concurrent` flag.
bunfig.toml

`--concurrent` CLI flag overrides this setting.
`test.onlyFailures`

When enabled, only failed tests are displayed in the output, which reduces noise in large test suites. Default `false`.
bunfig.toml

`--only-failures` flag.
`test.reporter`

Configure the test reporter settings.
`test.reporter.dots`

Enable the dots reporter, which prints one dot per test. Default `false`.
bunfig.toml

`test.reporter.junit`

Enable JUnit XML reporting and specify the output file path.
bunfig.toml

## Package manager

The`[install]` section configures the behavior of `bun install`.
bunfig.toml

`install.optional`

Whether to install optional dependencies. Default `true`.
bunfig.toml

`install.dev`

Whether to install development dependencies. Default `true`.
bunfig.toml

`install.peer`

Whether to install peer dependencies. Default `true`.
bunfig.toml

`install.production`

Whether `bun install` runs in “production mode”. Default `false`.
In production mode, `"devDependencies"` are not installed. The `--production` CLI flag overrides this setting.
bunfig.toml

`install.exact`

Whether to set an exact version in `package.json`. Default `false`.
By default Bun uses caret ranges; if the `latest` version of a package is `2.4.1`, Bun writes `^2.4.1` to your `package.json`, which accepts any version from `2.4.1` up to (but not including) `3.0.0`.
bunfig.toml

`install.ignoreScripts`

Whether to skip lifecycle scripts during install. Default `false`. Equivalent to the `--ignore-scripts` flag.
When `true`, Bun does not run any `preinstall` / `install` / `postinstall` / `prepare` scripts, both for your project and for packages in `trustedDependencies`. See [Lifecycle scripts](/docs/pm/lifecycle).

bunfig.toml

`install.concurrentScripts`

The maximum number of concurrent lifecycle scripts to run at once. Defaults to two times the number of CPU cores. Equivalent to the `--concurrent-scripts` flag.
bunfig.toml

`install.saveTextLockfile`

If `false`, `bun install` generates a binary `bun.lockb` instead of a text-based `bun.lock` file when no lockfile is present.
Default `true` (since Bun v1.2).
bunfig.toml

`install.auto`

Configure Bun’s [auto-install](/docs/runtime/auto-install)behavior. Default

`"auto"` — when no `node_modules` folder is found, Bun installs dependencies on the fly during execution.
bunfig.toml

`install.prefer`

Configure how Bun resolves package versions against the npm registry when running scripts. Default `"online"`.
bunfig.toml

`install.frozenLockfile`

When `true`, `bun install` does not update `bun.lock`. Default `false`. If `package.json` and the existing `bun.lock` disagree, the install errors.
bunfig.toml

`install.dryRun`

Whether `bun install` actually installs dependencies. Default `false`. When `true`, it’s equivalent to passing `--dry-run` to all `bun install` commands.
bunfig.toml

`install.globalDir`

The directory where Bun puts globally installed packages.
Environment variable: `BUN_INSTALL_GLOBAL_DIR`
bunfig.toml

`install.globalBinDir`

The directory where Bun links the binaries of globally installed packages.
Environment variable: `BUN_INSTALL_BIN`
bunfig.toml

`install.registry`

The default registry is `https://registry.npmjs.org/`. To change it:
bunfig.toml

`install.linkWorkspacePackages`

Whether to link workspace packages from the monorepo root to their respective `node_modules` directories. Default `true`.
bunfig.toml

`install.scopes`

To configure a registry for a particular scope (for example, `@myorg/<package>`), use `install.scopes`. You can reference environment variables with `$variable` notation.
bunfig.toml

`install.ca` and `install.cafile`

To configure a CA certificate, set `install.ca` to the certificate string or `install.cafile` to the path of a certificate file.
bunfig.toml

`install.cache`

To configure the cache behavior:
bunfig.toml

`install.lockfile`

Whether to generate a lockfile on `bun install`. Default `true`.
bunfig.toml

`bun.lock`. (A `bun.lock` is always created.) `"yarn"` is the only supported value.
bunfig.toml

`install.linker`

Configure the linker strategy: how `bun install` lays out dependencies in `node_modules`. Defaults to `"isolated"` for new workspaces, `"hoisted"` for new single-package projects and existing projects (made pre-v1.3.2).
See [Isolated installs](/docs/pm/isolated-installs).

bunfig.toml

`install.globalStore`

When using the `"isolated"` linker, share package installations across projects in a global virtual store at `<cache>/links/` and link `node_modules/.bun/<pkg>@<ver>` into it instead of materializing each package into the project. Makes warm installs after `rm -rf node_modules` an order of magnitude faster. Default `false`. Can also be set with the `BUN_INSTALL_GLOBAL_STORE` environment variable.
See [Global virtual store](/docs/pm/global-store).

bunfig.toml

`install.publicHoistPattern`

When using the `"isolated"` linker, packages matching these glob patterns are hoisted to the root `node_modules` directory so they can be resolved by any package in the project. Default `[]`. Similar to pnpm’s `public-hoist-pattern`.
bunfig.toml

`install.hoistPattern`

When using the `"isolated"` linker, packages matching these glob patterns are hoisted to the virtual store root (`node_modules/.bun`) so they can be resolved by other packages in the virtual store. Default `[]`. Similar to pnpm’s `hoist-pattern`.
bunfig.toml

`install.logLevel`

Set the log level for `bun install`. This can be one of `"debug"`, `"warn"`, or `"error"`.
bunfig.toml

`install.security.scanner`

Configure a security scanner to scan packages for vulnerabilities before installation.
First, install a security scanner from npm:
terminal

`@oven/bun-security-scanner` is an example package name, not a real package. Replace it with the scanner you want to
use, and consult that scanner’s documentation for the exact package name and installation instructions. Most scanners
are installed with `bun add`.`bunfig.toml`:
bunfig.toml

- Auto-install is automatically disabled for security
- Packages are scanned before installation
- Installation is cancelled if fatal issues are found
- Security warnings are displayed during installation

[using and writing security scanners](/docs/pm/security-scanner-api).

`install.minimumReleaseAge`

Configure a minimum age (in seconds) for npm package versions. Package versions published more recently than this threshold are filtered out during installation. Default `null` (disabled).
bunfig.toml

[Minimum release age](/docs/pm/cli/install#minimum-release-age).

`install.minimumReleaseAgeExcludes`

An array of package names that are exempt from the `minimumReleaseAge` check. Default `[]`.
bunfig.toml

`bun run`

The `[run]` section configures the `bun run` command. These settings also apply to the `bun` command when running a file, script, or executable.
For `bun run`, Bun only loads the local project’s `bunfig.toml` automatically (it doesn’t check for a global `.bunfig.toml`).
`run.shell` - use the system shell or Bun’s shell

The shell used to run `package.json` scripts with `bun run` or `bun`. Defaults to `"bun"` on Windows and `"system"` on other platforms.
To always use the system shell instead of Bun’s shell (the default everywhere except Windows):
bunfig.toml

bunfig.toml

`run.bun` - auto alias `node` to `bun`

When `true`, this prepends `$PATH` with a `node` symlink that points to the `bun` binary for all scripts or executables invoked by `bun run` or `bun`.
A script that runs `node` runs `bun` instead, with no changes to the script. This works recursively, so a script that runs another script that runs `node` also runs `bun`, and it applies to shebangs that point to `node`.
By default, this is enabled if `node` is not already in your `$PATH`.
bunfig.toml

`bun run` commands with `--bun`:
`false` to disable the `node` symlink.
`run.silent` - suppress reporting the command being run

When `true`, `bun run` and `bun` don’t print the command being run.
bunfig.toml

terminal

`--silent` to all `bun run` commands:
`run.elide-lines` - truncate filtered output

The number of lines of script output shown per script when using `--filter`. Default `10`. Set to `0` to show all lines. Equivalent to the `--elide-lines` flag.
bunfig.toml

`run.noOrphans` - don’t leave orphan processes behind

When `true`, Bun watches the process that spawned it and exits as soon as that parent goes away, even if the parent was force-killed and never got a chance to forward a signal. On its own exit, Bun also terminates every descendant process so nothing it spawned outlives it. Useful when Bun is launched by a supervisor (Electron, a CI runner, a thin shim) that may be force-killed.
On Linux this uses `prctl(PR_SET_PDEATHSIG)` and a `/proc` descendant walk; on macOS, `EVFILT_PROC`/`NOTE_EXIT` on the event loop’s kqueue and a libproc descendant walk; on Windows, a thread-pool wait on the parent’s process handle and a kill-on-close Job Object.
Equivalent to the `--no-orphans` CLI flag or the `BUN_FEATURE_FLAG_NO_ORPHANS=1` environment variable.
bunfig.toml

# Citations

1. Source page: https://bun.sh/docs/runtime/bunfig
