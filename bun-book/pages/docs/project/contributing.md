---
type: Web Page
title: Contributing - Bun
description: Contributing to Bun
resource: https://bun.sh/docs/project/contributing
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

[Building Windows](/docs/project/building-windows).

## Using Nix (Alternative)

The repository includes a Nix flake as an alternative to installing dependencies manually:`nix develop` provides all dependencies in an isolated, reproducible environment without requiring sudo.
## Install Dependencies (Manual)

Using your system’s package manager, install Bun’s dependencies:`rust-toolchain.toml`). Install Rust with [rustup](https://rustup.rs)rather than your distro’s

`rust`/`cargo` packages — the build scripts use rustup to automatically install and update the pinned nightly:
### Optional: Install `ccache`

`ccache` caches compilation artifacts, which speeds up rebuilds:
`ccache` automatically if it’s available. Check cache statistics with `ccache --show-stats`.
## Install LLVM

Bun requires LLVM 21.1.8 (`clang` is part of LLVM). The build system enforces this version: a mismatched version causes memory allocation failures at runtime. In most cases, you can install LLVM through your system package manager:
[manually](https://github.com/llvm/llvm-project/releases/tag/llvmorg-21.1.8). Make sure Clang/LLVM 21 is in your path:

## Building Bun

After cloning the repository, run the following command to build. This can take a while: it downloads and builds dependencies.`./build/debug/bun-debug`. It is recommended to add this to your `$PATH`. To verify the build worked, print its version:
## VSCode

VSCode is the recommended IDE for working on Bun; the repository includes configuration for it. After opening the repository, run`Extensions: Show Recommended Extensions` to install the recommended extensions for Rust and C++. rust-analyzer picks up the workspace `Cargo.toml` automatically and uses the pinned toolchain in `rust-toolchain.toml` for analysis, so diagnostics match the build.
If you use a different editor, point rust-analyzer (or your editor’s Rust plugin) at the repo root — the Cargo workspace and `rust-toolchain.toml` are discovered automatically.
We recommend adding `./build/debug` to your `$PATH` so that you can run `bun-debug` in your terminal:
## Running debug builds

The`bd` package.json script compiles and runs a debug build of Bun, only printing the output of the build process if it fails.
- Batch up your changes
- Use `cargo check -p <crate>`(or`bun run rust:check`for the whole workspace) to type-check Rust changes without linking.`bun run watch`runs`cargo check`on every save.
- Ensure rust-analyzer is running for inline diagnostics (the recommended VSCode extensions set this up)
- Prefer using the debugger (“CodeLLDB” in VSCode) to step through the code.
- Use debug logs. `BUN_DEBUG_<scope>=1`enables debug logging for the corresponding`declare_scope!(<scope>, ...)`/`scoped_log!(<scope>, ...)`logs. Set`BUN_DEBUG_QUIET_LOGS=1`to disable all debug logging that isn’t explicitly enabled. To dump debug logs into a file, set`BUN_DEBUG=<path-to-file>.log`. Debug logs are removed in release builds.
- src/js/**.ts changes rebuild almost instantly. Single-crate Rust changes and C++ changes are incremental; only the final link is unavoidable.

## Code generation scripts

Bun’s build process runs several code generation scripts automatically when certain files change:- `./src/codegen/generate-jssink.ts`— Generates- `build/debug/codegen/JSSink.cpp`,- `build/debug/codegen/JSSink.h`which implement various classes for interfacing with- `ReadableStream`. This is internally how- `FileSink`,- `ArrayBufferSink`,- `"type": "direct"`streams and other code related to streams work.
- `./src/codegen/generate-classes.ts`— Generates Rust & C++ bindings for JavaScriptCore classes implemented in Rust.- `**/*.classes.ts`files define the interfaces for classes, methods, prototypes, and getters/setters; the code generator reads them to generate the boilerplate that implements the JavaScript objects in C++ and wires them up to Rust.
- `./src/codegen/cppbind.ts`— Scans the C++ bindings for functions marked with an export attribute and generates automatic Rust FFI wrappers (- `cpp.rs`) for them.
- `./src/codegen/bundle-modules.ts`— Bundles built-in modules like- `node:fs`,- `bun:ffi`into files included in the final binary. In development, these can be reloaded without rebuilding native code (you still need to run- `bun run build`, but it re-reads the transpiled files from disk afterwards). In release builds, these are embedded into the binary.
- `./src/codegen/bundle-functions.ts`— Bundles globally-accessible functions implemented in JavaScript/TypeScript like- `ReadableStream`and- `WritableStream`. These are used similarly to the builtin modules, but the output more closely aligns with what WebKit/Safari does for Safari’s built-in functions, so implementations can be copy-pasted from WebKit as a starting point.

## Modifying ESM modules

Certain modules like`node:fs`, `node:stream`, `bun:sqlite`, and `ws` are implemented in JavaScript. These live in `src/js/{node,bun,thirdparty}` files and are pre-bundled using Bun.
## Release build

To compile a release build of Bun, run:`./build/release/bun` and `./build/release/bun-profile`.
### Download release build from pull requests

You can run the release build from a pull request without building it locally, which is useful for manually testing changes before they are merged. Use the`bun-pr` npm package:
`bun-pr` downloads the release build from the pull request’s GitHub Actions artifacts and adds it to `$PATH` as `bun-${pr-number}`, so you can run it directly:
`gh` CLI installed to authenticate with GitHub.
### Viewing CI failures from the terminal

Bun’s CI runs on BuildKite. Install the[BuildKite CLI](https://github.com/buildkite/cli)(

`brew install buildkite/buildkite/bk`) and set `BUILDKITE_API_TOKEN` to a read-scoped [API token](https://buildkite.com/user/api-access-tokens). The repo includes a

`.bk.yaml` so `bk` commands default to the `bun` pipeline.
`#1234` (PR number), a PR URL, a branch name, or a build number. Without one they use the current git branch.
## AddressSanitizer

[AddressSanitizer](https://en.wikipedia.org/wiki/AddressSanitizer)helps find memory issues, and is enabled by default in debug builds of Bun on Linux and macOS. This covers the Rust code, the C++ bindings, and all dependencies. It makes the build take about 2x longer; if that’s stopping you from being productive you can disable it with

`bun run build:debug:noasan` (or pass `--asan=off` to `scripts/build.ts`), but generally we recommend batching your changes up between builds.
To build a release build with AddressSanitizer, run:
## Building WebKit locally + Debug mode of JSC

WebKit is not cloned by default (to save time and disk space). To clone and build WebKit locally, run:`bun run build:local` handles everything: configuring JSC, building JSC, and building Bun. On subsequent runs, JSC rebuilds incrementally if any WebKit sources changed. `ninja -Cbuild/debug-local` also works after the first build, and builds both Bun and JSC.
The build output goes to `./build/debug-local` (instead of `./build/debug`), so you’ll need to update a couple of places:
- The first line in `src/js/builtins.d.ts`
- The `CompilationDatabase`line in`.clangd`config should be`CompilationDatabase: build/debug-local`
- In `.vscode/launch.json`, many configurations use`./build/debug/`, change them as you see fit

`C/C++: Select a Configuration` command so IntelliSense finds the debug headers.
If you make changes to Bun’s [WebKit fork](https://github.com/oven-sh/WebKit), you also have to change

`WEBKIT_VERSION` in `scripts/build/deps/webkit.ts` to point to your commit hash.
## Troubleshooting

### ’span’ file not found on Ubuntu

Clang uses`libstdc++`, the C++ standard library implementation provided by the GNU Compiler Collection (GCC), by default. Clang can link against `libc++` instead, but that requires explicitly passing the `-stdlib` flag.
Bun relies on C++20 features like `std::span`, which are not available in GCC versions lower than 11. As a result, running `bun run build` may fail with the following error:
`bun run build`, with Clang unable to compile a simple program:
### libarchive

If you see an error on macOS when compiling`libarchive`, run:
### macOS `library not found for -lSystem`

If you see this error when compiling, run:
### Cannot find `libatomic.a`

Bun defaults to linking `libatomic` statically, as not all systems have it. If you are building on a distro that does not have a static libatomic available, enable dynamic linking with:
## Using bun-debug

- Disable logging: `BUN_DEBUG_QUIET_LOGS=1 bun-debug ...`(to disable all debug logging)
- Enable logging for a specific scope: `BUN_DEBUG_EventLoop=1 bun-debug ...`(to enable`scoped_log!(EventLoop, ...)`output)
- Bun transpiles every file it runs. To see the actual executed source in a debug build, find it in `/tmp/bun-debug-src/...path/to/file`. For example, the transpiled version of`/home/bun/index.ts`is in`/tmp/bun-debug-src/home/bun/index.ts`

# Citations

1. Source page: https://bun.sh/docs/project/contributing
