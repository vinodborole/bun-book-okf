---
type: Web Page
title: License - Bun
description: License for Bun
resource: https://bun.sh/docs/project/license
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

## JavaScriptCore

Bun statically links JavaScriptCore (and WebKit), which is LGPL-2 licensed. WebCore files from WebKit are also licensed under LGPL-2. Per LGPL-2:(1) If you statically link against an LGPL’d library, you must also provide your application in an object (not necessarily source) format, so that a user has the opportunity to modify the library and relink the application.Bun’s patched version of WebKit lives at

[https://github.com/oven-sh/webkit](https://github.com/oven-sh/webkit). To relink Bun with changes:

- `git clone https://github.com/oven-sh/WebKit vendor/WebKit`
- `git -C vendor/WebKit checkout <commit>`(the commit hash in- `WEBKIT_VERSION`in- `scripts/build/deps/webkit.ts`)
- `bun run build:local`

`bun run build:local` compiles JavaScriptCore, compiles Bun’s `.cpp` bindings for JavaScriptCore (the object files that use JavaScriptCore), and outputs a new `bun` binary with your changes.
## Linked libraries

Bun statically links these libraries:| Library | License | 
|---|---|
| `boringssl` | [several licenses](https://boringssl.googlesource.com/boringssl/+/refs/heads/master/LICENSE) | 
| `brotli` | MIT | 
| `libarchive` | [several licenses](https://github.com/libarchive/libarchive/blob/master/COPYING) | 
| `lol-html` | BSD 3-Clause | 
| `mimalloc` | MIT | 
| `picohttp` | dual-licensed under the Perl License or the MIT License | 
| `zstd` | dual-licensed under the BSD License or GPLv2 license | 
| `simdutf` | Apache 2.0 | 
| `tinycc` | LGPL v2.1 | 
| `uSockets` | Apache 2.0 | 
| `zlib-ng` | zlib | 
| `c-ares` | MIT licensed | 
| 75`libicu` | [license here](https://github.com/unicode-org/icu/blob/main/icu4c/LICENSE) | 
| `libbase64` | BSD 2-Clause | 
| (on Windows)`libuv` | MIT | 
| `libdeflate` | MIT | 
| A fork of `uWebsockets` | Apache 2.0 licensed | 
| Parts of [Tigerbeetle’s IO code](https://github.com/tigerbeetle/tigerbeetle/blob/532c8b70b9142c17e07737ab6d3da68d7500cbca/src/io/windows.zig#L1) | Apache 2.0 licensed | 

## Polyfills

For compatibility, Bun embeds the following packages into its binary and injects them if imported.| Package | License | 
|---|---|
| `assert` | MIT | 
| `browserify-zlib` | MIT | 
| `buffer` | MIT | 
| `constants-browserify` | MIT | 
| `crypto-browserify` | MIT | 
| `domain-browser` | MIT | 
| `events` | MIT | 
| `https-browserify` | MIT | 
| `os-browserify` | MIT | 
| `path-browserify` | MIT | 
| `process` | MIT | 
| `punycode` | MIT | 
| `querystring-es3` | MIT | 
| `stream-browserify` | MIT | 
| `stream-http` | MIT | 
| `string_decoder` | MIT | 
| `timers-browserify` | MIT | 
| `tty-browserify` | MIT | 
| `url` | MIT | 
| `util` | MIT | 
| `vm-browserify` | MIT |

# Citations

1. Source page: https://bun.sh/docs/project/license
