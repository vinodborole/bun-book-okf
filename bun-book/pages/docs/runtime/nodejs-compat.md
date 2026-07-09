---
type: Web Page
title: Node.js Compatibility - Bun
description: Bun's compatibility status with Node.js APIs, modules, and globals
resource: https://bun.sh/docs/runtime/nodejs-compat
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`npm` packages intended for Node.js work with Bun. To ensure compatibility, we run thousands of tests from Node.js’ test suite before every release of Bun.
**If a package works in Node.js but doesn’t work in Bun, we consider it a bug in Bun.**

[Open an issue](https://bun.com/issues)and we’ll fix it. This page is updated regularly and reflects the latest version of Bun’s compatibility with

*Node.js v23*.

## Built-in Node.js modules

`node:assert`

🟢 Fully implemented.
`node:assert``node:buffer`

🟢 Fully implemented.
`node:buffer``node:console`

🟢 Fully implemented.
`node:console``node:dgram`

🟢 Fully implemented. > 90% of Node.js’s test suite passes.
`node:dgram``node:diagnostics_channel`

🟢 Fully implemented.
`node:diagnostics_channel``node:dns`

🟢 Fully implemented. > 90% of Node.js’s test suite passes.
`node:dns``node:events`

🟢 Fully implemented. 100% of Node.js’s test suite passes. `node:events``EventEmitterAsyncResource` uses `AsyncResource` underneath.
`node:fs`

🟢 Fully implemented. 92% of Node.js’s test suite passes.
`node:fs``node:http`

🟢 Fully implemented. The outgoing client request body is buffered instead of streamed.
`node:http``node:https`

🟢 APIs are implemented, but `node:https``Agent` is not always used.
`node:os`

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:os``node:path`

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:path``node:punycode`

🟢 Fully implemented. 100% of Node.js’s test suite passes, `node:punycode`*deprecated by Node.js*.

`node:querystring`

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:querystring``node:readline`

🟢 Fully implemented.
`node:readline``node:stream`

🟢 Fully implemented.
`node:stream``node:string_decoder`

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:string_decoder``node:timers`

🟢 Use the global `node:timers``setTimeout` and related functions instead.
`node:tty`

🟢 Fully implemented.
`node:tty``node:url`

🟢 Fully implemented.
`node:url``node:zlib`

🟢 Fully implemented. 98% of Node.js’s test suite passes.
`node:zlib``node:async_hooks`

🟡 `node:async_hooks``AsyncLocalStorage` and `AsyncResource` are implemented. v8 promise hooks are not called, and its usage is [strongly discouraged](https://nodejs.org/docs/latest/api/async_hooks.html#async-hooks).

`node:child_process`

🟡 Missing `node:child_process``proc.gid` `proc.uid`. `Stream` class not exported. IPC cannot send socket handles. Node.js ↔ Bun IPC can be used with JSON serialization.
`node:cluster`

🟡 Handles and file descriptors cannot be passed between workers, so load-balancing HTTP requests across processes is only supported on Linux (through `node:cluster``SO_REUSEPORT`). Otherwise, implemented but not battle-tested.
`node:crypto`

🟡 Missing `node:crypto``secureHeapUsed` `setEngine` `setFips`
`node:domain`

🟡 Missing `node:domain``Domain` `active`
`node:http2`

🟡 Client & server are implemented (95.25% of gRPC’s test suite passes).
`node:http2``node:module`

🟡 Missing `node:module``syncBuiltinESMExports`, `Module#load()`. Overriding `require.cache` is supported for ESM & CJS modules. `module._extensions`, `module._pathCache`, `module._cache` are no-ops. `module.register` is not implemented; we recommend [instead.](/docs/runtime/plugins)

`Bun.plugin``node:net`

🟢 Fully implemented.
`node:net``node:perf_hooks`

🟡 APIs are implemented, but the Node.js test suite for this module does not pass.
`node:perf_hooks``node:process`

🟡 See `node:process`[Global.](#process)

`process``node:sys`

🟡 See `node:sys`[.](#node-util)

`node:util``node:tls`

🟡 Missing `node:tls``tls.createSecurePair`.
`node:util`

🟡 Missing `node:util``getCallSite` `getCallSites` `getSystemErrorMap` `getSystemErrorMessage` `transferableAbortSignal` `transferableAbortController`
`node:v8`

🟡 `node:v8``writeHeapSnapshot` and `getHeapSnapshot` are implemented. `serialize` and `deserialize` use JavaScriptCore’s wire format instead of V8’s. Other methods are not implemented. For profiling, use [instead.](/docs/project/benchmarking#javascript-heap-stats)

`bun:jsc``node:vm`

🟡 Core functionality and ES modules are implemented, including `node:vm``vm.Script`, `vm.createContext`, `vm.runInContext`, `vm.runInNewContext`, `vm.runInThisContext`, `vm.compileFunction`, `vm.isContext`, `vm.Module`, `vm.SourceTextModule`, `vm.SyntheticModule`, and `importModuleDynamically` support. Options like `timeout` and `breakOnSigint` are fully supported.
`node:wasi`

🟡 Partially implemented.
`node:wasi``node:worker_threads`

🟡 `node:worker_threads``Worker` doesn’t support the following options: `stdin` `stdout` `stderr` `trackedUnmanagedFds` `resourceLimits`. Missing `markAsUntransferable` `moveMessagePortToContext`.
`node:inspector`

🟡 Partially implemented. The `node:inspector``Profiler` API is supported (`Profiler.enable`, `Profiler.disable`, `Profiler.start`, `Profiler.stop`, `Profiler.setSamplingInterval`). Other inspector APIs are not implemented.
`node:repl`

🔴 Not implemented.
`node:repl``node:sqlite`

🔴 Not implemented.
`node:sqlite``node:test`

🟡 Partially implemented. Missing `node:test``mock.module`, `mock.timers`, snapshots. Use [instead.](/docs/test)

`bun:test``node:trace_events`

🟢 Fully implemented.
`node:trace_events`## Node.js globals

The following list covers every global implemented by Node.js and Bun’s compatibility status for each.`AbortController`

🟢 Fully implemented.
`AbortController``AbortSignal`

🟢 Fully implemented.
`AbortSignal``Blob`

🟢 Fully implemented.
`Blob``Buffer`

🟢 Fully implemented.
`Buffer``ByteLengthQueuingStrategy`

🟢 Fully implemented.
`ByteLengthQueuingStrategy``__dirname`

🟢 Fully implemented.
`__dirname``__filename`

🟢 Fully implemented.
`__filename``atob()`

🟢 Fully implemented.
`atob()``Atomics`

🟢 Fully implemented.
`Atomics``BroadcastChannel`

🟢 Fully implemented.
`BroadcastChannel``btoa()`

🟢 Fully implemented.
`btoa()``clearImmediate()`

🟢 Fully implemented.
`clearImmediate()``clearInterval()`

🟢 Fully implemented.
`clearInterval()``clearTimeout()`

🟢 Fully implemented.
`clearTimeout()``CompressionStream`

🟢 Fully implemented.
`CompressionStream``console`

🟢 Fully implemented.
`console``CountQueuingStrategy`

🟢 Fully implemented.
`CountQueuingStrategy``Crypto`

🟢 Fully implemented.
`Crypto``SubtleCrypto (crypto)`

🟢 Fully implemented.
`SubtleCrypto (crypto)``CryptoKey`

🟢 Fully implemented.
`CryptoKey``CustomEvent`

🟢 Fully implemented.
`CustomEvent``DecompressionStream`

🟢 Fully implemented.
`DecompressionStream``Event`

🟢 Fully implemented.
`Event``EventTarget`

🟢 Fully implemented.
`EventTarget``exports`

🟢 Fully implemented.
`exports``fetch`

🟢 Fully implemented.
`fetch``FormData`

🟢 Fully implemented.
`FormData``global`

🟢 Implemented. `global``global` is an object containing all objects in the global namespace. It’s rarely referenced directly, as its contents are available without a prefix, for example `__dirname` instead of `global.__dirname`.
`globalThis`

🟢 Aliases to `globalThis``global`.
`Headers`

🟢 Fully implemented.
`Headers``MessageChannel`

🟢 Fully implemented.
`MessageChannel``MessageEvent`

🟢 Fully implemented.
`MessageEvent``MessagePort`

🟢 Fully implemented.
`MessagePort``module`

🟢 Fully implemented.
`module``PerformanceEntry`

🟢 Fully implemented.
`PerformanceEntry``PerformanceMark`

🟢 Fully implemented.
`PerformanceMark``PerformanceMeasure`

🟢 Fully implemented.
`PerformanceMeasure``PerformanceObserver`

🟢 Fully implemented.
`PerformanceObserver``PerformanceObserverEntryList`

🟢 Fully implemented.
`PerformanceObserverEntryList``PerformanceResourceTiming`

🟢 Fully implemented.
`PerformanceResourceTiming``performance`

🟢 Fully implemented.
`performance``process`

🟡 Mostly implemented. `process``process.binding` (internal Node.js bindings some packages rely on) is partially implemented. `process.title` is a no-op on macOS & Linux. `getActiveResourcesInfo` `setActiveResourcesInfo`, `getActiveResources` and `setSourceMapsEnabled` are stubs. Newer APIs like `process.loadEnvFile` are not implemented.
`queueMicrotask()`

🟢 Fully implemented.
`queueMicrotask()``ReadableByteStreamController`

🟢 Fully implemented.
`ReadableByteStreamController``ReadableStream`

🟢 Fully implemented.
`ReadableStream``ReadableStreamBYOBReader`

🟢 Fully implemented.
`ReadableStreamBYOBReader``ReadableStreamBYOBRequest`

🟢 Fully implemented.
`ReadableStreamBYOBRequest``ReadableStreamDefaultController`

🟢 Fully implemented.
`ReadableStreamDefaultController``ReadableStreamDefaultReader`

🟢 Fully implemented.
`ReadableStreamDefaultReader``require()`

🟢 Fully implemented, including `require()`[,](https://nodejs.org/api/modules.html#requiremain)

`require.main`[,](https://nodejs.org/api/modules.html#requirecache)

`require.cache`[.](https://nodejs.org/api/modules.html#requireresolverequest-options)

`require.resolve``Response`

🟢 Fully implemented.
`Response``Request`

🟢 Fully implemented.
`Request``setImmediate()`

🟢 Fully implemented.
`setImmediate()``setInterval()`

🟢 Fully implemented.
`setInterval()``setTimeout()`

🟢 Fully implemented.
`setTimeout()``structuredClone()`

🟢 Fully implemented.
`structuredClone()``SubtleCrypto`

🟢 Fully implemented.
`SubtleCrypto``DOMException`

🟢 Fully implemented.
`DOMException``TextDecoder`

🟢 Fully implemented.
`TextDecoder``TextDecoderStream`

🟢 Fully implemented.
`TextDecoderStream``TextEncoder`

🟢 Fully implemented.
`TextEncoder``TextEncoderStream`

🟢 Fully implemented.
`TextEncoderStream``TransformStream`

🟢 Fully implemented.
`TransformStream``TransformStreamDefaultController`

🟢 Fully implemented.
`TransformStreamDefaultController``URL`

🟢 Fully implemented.
`URL``URLSearchParams`

🟢 Fully implemented.
`URLSearchParams``WebAssembly`

🟢 Fully implemented.
`WebAssembly``WritableStream`

🟢 Fully implemented.
`WritableStream``WritableStreamDefaultController`

🟢 Fully implemented.
`WritableStreamDefaultController``WritableStreamDefaultWriter`

🟢 Fully implemented.`WritableStreamDefaultWriter`

# Citations

1. Source page: https://bun.sh/docs/runtime/nodejs-compat
