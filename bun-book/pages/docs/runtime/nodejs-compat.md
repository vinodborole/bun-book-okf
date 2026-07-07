---
type: Web Page
title: Node.js Compatibility - Bun
description: Bun's compatibility status with Node.js APIs, modules, and globals
resource: https://bun.sh/docs/runtime/nodejs-compat
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`npm` packages intended for Node.js work with Bun. To ensure compatibility, we run thousands of tests from Node.js’ test suite before every release of Bun.
**If a package works in Node.js but doesn’t work in Bun, we consider it a bug in Bun.**Open an issue and we’ll fix it. This page is updated regularly and reflects the latest version of Bun’s compatibility with

*Node.js v23*.

## Built-in Node.js modules

`node:assert`

🟢 Fully implemented.
`node:buffer`

🟢 Fully implemented.
`node:console`

🟢 Fully implemented.
`node:dgram`

🟢 Fully implemented. > 90% of Node.js’s test suite passes.
`node:diagnostics_channel`

🟢 Fully implemented.
`node:dns`

🟢 Fully implemented. > 90% of Node.js’s test suite passes.
`node:events`

🟢 Fully implemented. 100% of Node.js’s test suite passes. `EventEmitterAsyncResource` uses `AsyncResource` underneath.
`node:fs`

🟢 Fully implemented. 92% of Node.js’s test suite passes.
`node:http`

🟢 Fully implemented. The outgoing client request body is buffered instead of streamed.
`node:https`

🟢 APIs are implemented, but `Agent` is not always used.
`node:os`

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:path`

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:punycode`

🟢 Fully implemented. 100% of Node.js’s test suite passes, *deprecated by Node.js*.

`node:querystring`

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:readline`

🟢 Fully implemented.
`node:stream`

🟢 Fully implemented.
`node:string_decoder`

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:timers`

🟢 Use the global `setTimeout` and related functions instead.
`node:tty`

🟢 Fully implemented.
`node:url`

🟢 Fully implemented.
`node:zlib`

🟢 Fully implemented. 98% of Node.js’s test suite passes.
`node:async_hooks`

🟡 `AsyncLocalStorage` and `AsyncResource` are implemented. v8 promise hooks are not called, and its usage is strongly discouraged.
`node:child_process`

🟡 Missing `proc.gid` `proc.uid`. `Stream` class not exported. IPC cannot send socket handles. Node.js ↔ Bun IPC can be used with JSON serialization.
`node:cluster`

🟡 Handles and file descriptors cannot be passed between workers, so load-balancing HTTP requests across processes is only supported on Linux (through `SO_REUSEPORT`). Otherwise, implemented but not battle-tested.
`node:crypto`

🟡 Missing `secureHeapUsed` `setEngine` `setFips`
`node:domain`

🟡 Missing `Domain` `active`
`node:http2`

🟡 Client & server are implemented (95.25% of gRPC’s test suite passes). Missing `options.allowHTTP1`, `options.enableConnectProtocol`, ALTSVC extension, and `http2stream.pushStream`.
`node:module`

🟡 Missing `syncBuiltinESMExports`, `Module#load()`. Overriding `require.cache` is supported for ESM & CJS modules. `module._extensions`, `module._pathCache`, `module._cache` are no-ops. `module.register` is not implemented; we recommend `Bun.plugin` instead.
`node:net`

🟢 Fully implemented.
`node:perf_hooks`

🟡 APIs are implemented, but the Node.js test suite for this module does not pass.
`node:process`

🟡 See `process` Global.
`node:sys`

🟡 See `node:util`.
`node:tls`

🟡 Missing `tls.createSecurePair`.
`node:util`

🟡 Missing `getCallSite` `getCallSites` `getSystemErrorMap` `getSystemErrorMessage` `transferableAbortSignal` `transferableAbortController`
`node:v8`

🟡 `writeHeapSnapshot` and `getHeapSnapshot` are implemented. `serialize` and `deserialize` use JavaScriptCore’s wire format instead of V8’s. Other methods are not implemented. For profiling, use `bun:jsc` instead.
`node:vm`

🟡 Core functionality and ES modules are implemented, including `vm.Script`, `vm.createContext`, `vm.runInContext`, `vm.runInNewContext`, `vm.runInThisContext`, `vm.compileFunction`, `vm.isContext`, `vm.Module`, `vm.SourceTextModule`, `vm.SyntheticModule`, and `importModuleDynamically` support. Options like `timeout` and `breakOnSigint` are fully supported. Missing `vm.measureMemory` and some `cachedData` functionality.
`node:wasi`

🟡 Partially implemented.
`node:worker_threads`

🟡 `Worker` doesn’t support the following options: `stdin` `stdout` `stderr` `trackedUnmanagedFds` `resourceLimits`. Missing `markAsUntransferable` `moveMessagePortToContext`.
`node:inspector`

🟡 Partially implemented. The `Profiler` API is supported (`Profiler.enable`, `Profiler.disable`, `Profiler.start`, `Profiler.stop`, `Profiler.setSamplingInterval`). Other inspector APIs are not implemented.
`node:repl`

🔴 Not implemented.
`node:sqlite`

🔴 Not implemented.
`node:test`

🟡 Partially implemented. Missing mocks, snapshots, timers. Use `bun:test` instead.
`node:trace_events`

🔴 Not implemented.
## Node.js globals

The following list covers every global implemented by Node.js and Bun’s compatibility status for each.`AbortController`

🟢 Fully implemented.
`AbortSignal`

🟢 Fully implemented.
`Blob`

🟢 Fully implemented.
`Buffer`

🟢 Fully implemented.
`ByteLengthQueuingStrategy`

🟢 Fully implemented.
`__dirname`

🟢 Fully implemented.
`__filename`

🟢 Fully implemented.
`atob()`

🟢 Fully implemented.
`Atomics`

🟢 Fully implemented.
`BroadcastChannel`

🟢 Fully implemented.
`btoa()`

🟢 Fully implemented.
`clearImmediate()`

🟢 Fully implemented.
`clearInterval()`

🟢 Fully implemented.
`clearTimeout()`

🟢 Fully implemented.
`CompressionStream`

🟢 Fully implemented.
`console`

🟢 Fully implemented.
`CountQueuingStrategy`

🟢 Fully implemented.
`Crypto`

🟢 Fully implemented.
`SubtleCrypto (crypto)`

🟢 Fully implemented.
`CryptoKey`

🟢 Fully implemented.
`CustomEvent`

🟢 Fully implemented.
`DecompressionStream`

🟢 Fully implemented.
`Event`

🟢 Fully implemented.
`EventTarget`

🟢 Fully implemented.
`exports`

🟢 Fully implemented.
`fetch`

🟢 Fully implemented.
`FormData`

🟢 Fully implemented.
`global`

🟢 Implemented. `global` is an object containing all objects in the global namespace. It’s rarely referenced directly, as its contents are available without a prefix, for example `__dirname` instead of `global.__dirname`.
`globalThis`

🟢 Aliases to `global`.
`Headers`

🟢 Fully implemented.
`MessageChannel`

🟢 Fully implemented.
`MessageEvent`

🟢 Fully implemented.
`MessagePort`

🟢 Fully implemented.
`module`

🟢 Fully implemented.
`PerformanceEntry`

🟢 Fully implemented.
`PerformanceMark`

🟢 Fully implemented.
`PerformanceMeasure`

🟢 Fully implemented.
`PerformanceObserver`

🟢 Fully implemented.
`PerformanceObserverEntryList`

🟢 Fully implemented.
`PerformanceResourceTiming`

🟢 Fully implemented.
`performance`

🟢 Fully implemented.
`process`

🟡 Mostly implemented. `process.binding` (internal Node.js bindings some packages rely on) is partially implemented. `process.title` is a no-op on macOS & Linux. `getActiveResourcesInfo` `setActiveResourcesInfo`, `getActiveResources` and `setSourceMapsEnabled` are stubs. Newer APIs like `process.loadEnvFile` and `process.getBuiltinModule` are not implemented.
`queueMicrotask()`

🟢 Fully implemented.
`ReadableByteStreamController`

🟢 Fully implemented.
`ReadableStream`

🟢 Fully implemented.
`ReadableStreamBYOBReader`

🟢 Fully implemented.
`ReadableStreamBYOBRequest`

🟢 Fully implemented.
`ReadableStreamDefaultController`

🟢 Fully implemented.
`ReadableStreamDefaultReader`

🟢 Fully implemented.
`require()`

🟢 Fully implemented, including `require.main`, `require.cache`, `require.resolve`.

# Citations

1. Source page: https://bun.sh/docs/runtime/nodejs-compat
