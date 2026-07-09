---
type: Web Page
title: Building Windows - Bun
description: Building Bun on Windows
resource: https://bun.sh/docs/project/building-windows
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

[PowerShell 7 (](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.4)instead of the default

`pwsh.exe`)`powershell.exe`. If you run into problems, ask in the [#contributing channel on our Discord](http://bun.com/discord).

## Prerequisites

### Enable Scripts

By default, running unverified scripts is blocked.### System Dependencies

Bun v1.1 or later. The build uses Bun to run its own code generators.[Visual Studio](https://visualstudio.microsoft.com)with the “Desktop Development with C++” workload. While installing, also install Git if Git for Windows is not already installed. Install Visual Studio with the graphical wizard or through WinGet:

- LLVM 21.1.8
- Go
- Rust (via rustup)
- NASM
- Perl
- Ruby
- Node.js

rustup installs the Rust nightly toolchain pinned in 

`rust-toolchain.toml` on the first build.[Scoop](https://scoop.sh)to install these remaining tools.

Scoop (x64)

ARM64

Do not install these with WinGet or another package manager: you will likely get Strawberry Perl instead of a more
minimal installation of Perl. Strawberry Perl adds many other utilities to 

`$Env:PATH` that conflict with MSVC and
break the build.Scoop

ARM64 builds do not need Cygwin because WebKit is provided as a pre-built binary.

**use a PowerShell terminal with**. Load the script by running it:

`.\scripts\vs-shell.ps1` sourced`mt.exe`:
Avoid installing 

`ninja` / `cmake` into your global path: you may end up building Bun without `.\scripts\vs-shell.ps1`
sourced.## Building

`bun-debug.exe` to the `build/debug` folder.
`$Env:PATH`: open the Start menu, type “Path”, and use the environment variables menu to add `C:\.....\bun\build\debug` to the user environment variable `PATH`. Then restart your editor (if it still does not pick up the change, log out and log back in).
## Extra paths

- WebKit is extracted to `build/debug/cache/webkit/`

## Tests

Run the test suite with`bun test <path>` or with the wrapper script `bun node:test <path>`. The `bun node:test` command runs every test file in a separate instance of `bun.exe`, so a crash in the test runner does not stop the entire suite.
## Troubleshooting

### .rc file fails to build

`llvm-rc.exe` is odd; don’t use it. Use `rc.exe` instead: make sure you are in a Visual Studio dev terminal, and check `rc /?` to confirm it is `Microsoft Resource Compiler`.
### failed to write output ‘bun-debug.exe’: permission denied

You cannot overwrite`bun-debug.exe` while it is open. You likely have a running instance, maybe in the VS Code debugger.
## Cross-compiling from Linux

You can also build Windows binaries (both x64 and arm64) on a Linux host. The build uses the host LLVM’s`clang-cl`, `lld-link`, `llvm-lib` and `llvm-rc` (part of every LLVM distribution), plus an “xwin splat” of the MSVC CRT/STL and Windows SDK for headers and import libraries.
### Prerequisites

- The same LLVM version a native build uses (see `scripts/bootstrap.sh``llvm_version_exact`), installed so that`clang-cl`,`lld-link`,`llvm-lib`and`llvm-rc`are available. On Debian/Ubuntu,`apt.llvm.org`packages provide all of them.
- `nasm`(only needed for Windows x64; BoringSSL’s x64 assembly is NASM syntax).
- Rust std for the Windows targets (`rust-toolchain.toml`lists them;`rustup target add x86_64-pc-windows-msvc aarch64-pc-windows-msvc`if missing).
- A Windows sysroot: an [xwin](https://github.com/Jake-Shadle/xwin)splat of the MSVC CRT, Windows SDK, and ATL laid out like a Visual Studio install. Downloading these components means accepting Microsoft’s license terms for them.

`/opt/winsysroot` (or `/opt/xwin`) automatically; elsewhere, set `WINDOWS_SYSROOT=<path>` or pass `--winsysroot=<path>` (a user-writable path also lets configure manage the aliases for you). Configure validates the splat at the start of every cross build. CI agents bake the same splat into their images (`.buildkite/Dockerfile`, `scripts/bootstrap.sh`); when an agent doesn’t have one, the build fetches it into its cache dir at configure time.
### Building

`build/debug-windows-x64/bun-debug.exe`, `build/release-windows-aarch64/bun-profile.exe` + `bun.exe`, and so on. Equivalent raw flags: `bun run build --os=windows --arch=aarch64`.
Cross-compiled executables are not run on the host (the `--revision` smoke test is skipped), so test them on a Windows machine or under Wine.
### LTO

x64 release cross builds support ThinLTO with cross-language (Rust↔C++) LTO. It’s opt-in:`--lto=on` compiles Bun’s C/C++ with `-flto=thin`, makes rustc emit LLVM bitcode (`-Clinker-plugin-lto`), pulls the `bun-webkit-windows-amd64-lto` ThinLTO prebuilt, and links everything with rustc’s bundled `lld-link` (its LLVM is new enough to read both compilers’ bitcode). There is no LTO for arm64 (no `-lto` WebKit prebuilt: LLVM’s CodeView emitter can’t handle ARM64 NEON tuple registers during LTO codegen) or for `--baseline`.

# Citations

1. Source page: https://bun.sh/docs/project/building-windows
