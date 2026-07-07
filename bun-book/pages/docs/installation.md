---
type: Web Page
title: Installation - Bun
description: Install Bun with npm, Homebrew, Docker, or the official script.
resource: https://bun.sh/docs/installation
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

## Overview

Bun ships as a single, dependency-free executable. Install it with the install script, a package manager, or Docker on macOS, Linux, and Windows.## Installation

- macOS & Linux
- Windows
- Package Managers
- Docker

**Linux users**The

`unzip` package is required to install Bun (`sudo apt install unzip`). Kernel version 5.6 or higher is recommended; Bun runs on kernels as old as 3.10 (RHEL 7) with graceful degradation of newer syscalls. Use `uname -r` to check your kernel version.terminal

Add Bun to your PATH

Add Bun to your PATH

## Upgrading

Once installed, the binary can upgrade itself:terminal

## Canary Builds

-> View canary build Bun automatically releases an (untested) canary build on every commit to main. To upgrade to the latest canary build:terminal

## Installing Older Versions

Since Bun is a single binary, you can install older versions by re-running the installer script with a specific version.- Linux & macOS
- Windows

To install a specific version, pass the git tag to the install script:

terminal

## Direct Downloads

To download Bun binaries directly, visit the releases page on GitHub.### Latest Version Downloads

## Linux x64

Standard Linux x64 binary

## Linux x64 Baseline

For older CPUs without AVX2

## Windows x64

Standard Windows binary

## Windows x64 Baseline

For older CPUs without AVX2

## Windows ARM64

Windows on ARM (Snapdragon, etc.)

## macOS ARM64

Apple Silicon (M1/M2/M3)

## macOS x64

Intel Macs

## Linux ARM64

ARM64 Linux systems

### Musl Binaries

For distributions without`glibc` (Alpine Linux, Void Linux):
Bun’s glibc binaries require glibc 2.17 or newer. If you encounter an error like 

`bun:     /lib/x86_64-linux-gnu/libc.so.6: version GLIBC_... not found`, try using the musl binary. Bun’s install script
automatically chooses the correct binary for your system.## CPU Requirements

CPU requirements depend on which binary you’re using:- Standard Builds
- Baseline Builds

**x64 binaries**target the Haswell CPU architecture (AVX and AVX2 instructions required)

| Platform | Intel Requirement | AMD Requirement | 
|---|---|---|
| x64 | Haswell (4th gen Core) or newer | Excavator or newer | 

Bun does not support CPUs older than the baseline target, which requires the SSE4.2 extension. Bun requires macOS 13.0
or later.

## Uninstall

To remove Bun from your system:- macOS & Linux
- Windows
- Package Managers

terminal

# Citations

1. Source page: https://bun.sh/docs/installation
