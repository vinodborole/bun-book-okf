---
type: Web Page
title: Installation | Bun Docs
description: Install Bun with npm, Homebrew, Docker, or the official script.
resource: https://bun.sh/docs/installation
timestamp: '2026-08-24T06:34:12.842221+00:00'
---

# Installation

Install Bun with npm, Homebrew, Docker, or the official script.

## Overview

Bun ships as a single, dependency-free executable. Install it with the install script, a package manager, or Docker on macOS, Linux, and Windows.

`bun --version` and `bun --revision`.
## Installation

`curl -fsSL https://bun.com/install | bash`
**Linux users:** You need the `unzip` package to install Bun (`sudo apt install unzip`). We recommend kernel version 5.6 or higher. Bun runs on kernels as old as 3.10 (RHEL 7) with graceful degradation of newer syscalls. Use `uname -r` to check your kernel version.

`powershell -c "irm bun.sh/install.ps1|iex"`
Bun requires Windows 10 version 1809 or later.

For support and discussion, join the **#windows** channel on the [Discord](https://bun.com/discord).

``npm install -g bun # the last `npm` command you'll ever need```brew install oven-sh/bun/bun``scoop install bun`
Bun provides a Docker image that supports both Linux x64 and arm64.

```
docker pull oven/bun
docker run --rm --init --ulimit memlock=-1:-1 oven/bun
```
### Image Variants

Bun also publishes image variants for different operating systems:

```
docker pull oven/bun:debian
docker pull oven/bun:slim
docker pull oven/bun:distroless
docker pull oven/bun:alpine
```
To check that Bun was installed successfully, open a new terminal window and run:

```
bun --version
# Output: 1.x.y
# See the precise commit of `oven-sh/bun` that you're using
bun --revision
# Output: 1.x.y+b7982ac13189
```
If you've installed Bun but are seeing a `command not found` error, you may have to manually add the installation
directory (`~/.bun/bin`) to your `PATH`.

## Add Bun to your PATH

Determine which shell you're using

```
echo $SHELL
# /bin/zsh  or /bin/bash or /bin/fish
```
Open your shell configuration file

- For bash: `~/.bashrc`
- For zsh: `~/.zshrc`
- For fish: `~/.config/fish/config.fish`

Add the Bun directory to PATH

Add these lines to your configuration file:

```
export BUN_INSTALL="$HOME/.bun"
export PATH="$BUN_INSTALL/bin:$PATH"
```
Reload your shell configuration

`source ~/.bashrc  # or ~/.zshrc`
Determine if the bun binary is properly installed

`& "$env:USERPROFILE\.bun\bin\bun" --version`
If the command runs successfully but `bun --version` is not recognized, bun is not in your system's PATH. To fix this, open a PowerShell terminal and run the following command:

```
[System.Environment]::SetEnvironmentVariable(
  "Path",
  [System.Environment]::GetEnvironmentVariable("Path", "User") + ";$env:USERPROFILE\.bun\bin",
  [System.EnvironmentVariableTarget]::User
)
```
Restart your terminal

Restart your terminal and test with `bun --version`.

`bun --version`
## Upgrading

Once installed, the binary can upgrade itself:

`bun upgrade`
**Homebrew users** 

To avoid conflicts with Homebrew, use `brew upgrade bun` instead.

**Scoop users** 

To avoid conflicts with Scoop, use `scoop update bun` instead.

## Canary Builds

Bun automatically releases an (untested) canary build on every commit to main. To upgrade to the latest canary build:

```
# Upgrade to latest canary
bun upgrade --canary
# Switch back to stable
bun upgrade --stable
```
Use a canary build to test new features and bug fixes before they reach a stable release. To help the Bun team fix bugs faster, canary builds automatically upload crash reports.

## Installing Older Versions

Since Bun is a single binary, you can install older versions by re-running the installer script with a specific version.

To install a specific version, pass the git tag to the install script:

`curl -fsSL https://bun.com/install | bash -s "bun-v1.3.3"`
On Windows, pass the version number to the PowerShell install script:

`iex "& {$(irm https://bun.com/install.ps1)} -Version 1.3.3"`
## Direct Downloads

To download Bun binaries directly, visit the [releases page on GitHub](https://github.com/oven-sh/bun/releases).

### Latest Version Downloads

### Musl Binaries

For distributions without `glibc` (Alpine Linux, Void Linux):

Bun's glibc binaries require glibc 2.17 or newer. If you encounter an error like ```
bun:
  /lib/x86_64-linux-gnu/libc.so.6: version GLIBC_... not found
```
, try using the musl binary. Bun's install script
automatically chooses the correct binary for your system.

## CPU Requirements

Bun ships a single x64 binary per platform. It targets the Nehalem microarchitecture (SSE4.2) and selects AVX2/AVX-512 code paths at runtime when the CPU supports them, so there is no separate "baseline" download to choose.

| Platform | Intel Requirement | AMD Requirement | 
|---|---|---|
| x64 | Nehalem (1st gen Core) or newer | Bulldozer or newer | 

Bun does not support x64 CPUs without the SSE4.2 extension. Bun requires macOS 13.0 or later. The `-baseline` release
assets and `@oven/bun-*-x64-baseline` npm packages are kept as aliases of the single x64 binary for backward
compatibility with older install scripts.

## Uninstall

To remove Bun from your system:

`rm -rf ~/.bun``powershell -c ~\.bun\uninstall.ps1``npm uninstall -g bun``brew uninstall bun``scoop uninstall bun`

# Citations

1. Source page: https://bun.sh/docs/installation
