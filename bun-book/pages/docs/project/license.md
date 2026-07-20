---
type: Web Page
title: License - Bun
description: License for Bun
resource: https://bun.sh/docs/project/license
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

## JavaScriptCore

Bun statically links JavaScriptCore (and WebKit), which is LGPL-2 licensed. WebCore files from WebKit are also licensed under LGPL-2. Per LGPL-2:(1) If you statically link against an LGPL’d library, you must also provide your application in an object (not necessarily source) format, so that a user has the opportunity to modify the library and relink the application.Bun’s patched version of WebKit lives at

[https://github.com/oven-sh/webkit](https://github.com/oven-sh/webkit). To relink Bun with changes:

- `git clone https://github.com/oven-sh/WebKit vendor/WebKit`
- `bun sync-webkit-source`(checks out the version pinned in- `WEBKIT_VERSION`in- `scripts/build/deps/webkit.ts`)
- `bun run build:local`

`bun run build:local` compiles JavaScriptCore, compiles Bun’s `.cpp` bindings for JavaScriptCore (the object files that use JavaScriptCore), and outputs a new `bun` binary with your changes.

# Citations

1. Source page: https://bun.sh/docs/project/license
