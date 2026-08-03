---
type: Web Page
title: Node.js Compatibility - Bun
description: Bun's compatibility status with Node.js APIs, modules, and globals
resource: https://bun.sh/docs/runtime/nodejs-compat
timestamp: '2026-08-03T08:59:43.078871+00:00'
---

`npm` packages intended for Node.js work with Bun. To ensure compatibility, we run thousands of tests from Node.js’ test suite before every release of Bun.
**If a package works in Node.js but doesn’t work in Bun, we consider it a bug in Bun.**

[Open an issue](https://bun.com/issues)and we’ll fix it. This page is updated regularly and reflects the latest version of Bun’s compatibility with

*Node.js v23*.

## Built-in Node.js modules

### [`node:assert`](https://nodejs.org/api/assert.html)

🟢 Fully implemented.
`node:assert`
### [`node:buffer`](https://nodejs.org/api/buffer.html)

🟢 Fully implemented.
`node:buffer`
### [`node:console`](https://nodejs.org/api/console.html)

🟢 Fully implemented.
`node:console`
### [`node:dgram`](https://nodejs.org/api/dgram.html)

🟢 Fully implemented. > 90% of Node.js’s test suite passes.
`node:dgram`
### [`node:diagnostics_channel`](https://nodejs.org/api/diagnostics_channel.html)

🟢 Fully implemented.
`node:diagnostics_channel`
### [`node:dns`](https://nodejs.org/api/dns.html)

🟢 Fully implemented. > 90% of Node.js’s test suite passes.
`node:dns`
### [`node:events`](https://nodejs.org/api/events.html)

🟢 Fully implemented. 100% of Node.js’s test suite passes. `node:events``EventEmitterAsyncResource` uses `AsyncResource` underneath.
### [`node:fs`](https://nodejs.org/api/fs.html)

🟢 Fully implemented. 92% of Node.js’s test suite passes.
`node:fs`
### [`node:http`](https://nodejs.org/api/http.html)

🟢 Fully implemented. The outgoing client request body is buffered instead of streamed.
`node:http`
### [`node:https`](https://nodejs.org/api/https.html)

🟢 APIs are implemented, but `node:https``Agent` is not always used.
### [`node:os`](https://nodejs.org/api/os.html)

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:os`
### [`node:path`](https://nodejs.org/api/path.html)

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:path`
### [`node:punycode`](https://nodejs.org/api/punycode.html)

🟢 Fully implemented. 100% of Node.js’s test suite passes, `node:punycode`
*deprecated by Node.js*.

### [`node:querystring`](https://nodejs.org/api/querystring.html)

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:querystring`
### [`node:readline`](https://nodejs.org/api/readline.html)

🟢 Fully implemented.
`node:readline`
### [`node:stream`](https://nodejs.org/api/stream.html)

🟢 Fully implemented.
`node:stream`
### [`node:string_decoder`](https://nodejs.org/api/string_decoder.html)

🟢 Fully implemented. 100% of Node.js’s test suite passes.
`node:string_decoder`
### [`node:timers`](https://nodejs.org/api/timers.html)

🟢 Use the global `node:timers``setTimeout` and related functions instead.
### [`node:tty`](https://nodejs.org/api/tty.html)

🟢 Fully implemented.
`node:tty`
### [`node:url`](https://nodejs.org/api/url.html)

🟢 Fully implemented.
`node:url`
### [`node:zlib`](https://nodejs.org/api/zlib.html)

🟢 Fully implemented. 98% of Node.js’s test suite passes.
`node:zlib`
### [`node:async_hooks`](https://nodejs.org/api/async_hooks.html)

🟡 `node:async_hooks``AsyncLocalStorage` and `AsyncResource` are implemented. v8 promise hooks are not called, and its usage is [strongly discouraged](https://nodejs.org/docs/latest/api/async_hooks.html#async-hooks).

### [`node:child_process`](https://nodejs.org/api/child_process.html)

🟡 Missing `node:child_process``proc.gid` `proc.uid`. `Stream` class not exported. IPC cannot send socket handles. Node.js ↔ Bun IPC can be used with JSON serialization.
### [`node:cluster`](https://nodejs.org/api/cluster.html)

🟡 Handles and file descriptors cannot be passed between workers, so load-balancing HTTP requests across processes is only supported on Linux (through `node:cluster``SO_REUSEPORT`). Otherwise, implemented but not battle-tested.
### [`node:crypto`](https://nodejs.org/api/crypto.html)

🟡 Missing `node:crypto``secureHeapUsed` `setEngine` `setFips`
### [`node:domain`](https://nodejs.org/api/domain.html)

🟡 Missing `node:domain``Domain` `active`
### [`node:http2`](https://nodejs.org/api/http2.html)

🟡 Client & server are implemented (95.25% of gRPC’s test suite passes).
`node:http2`
### [`node:module`](https://nodejs.org/api/module.html)

🟡 Missing `node:module``syncBuiltinESMExports`, `Module#load()`. Overriding `require.cache` is supported for ESM & CJS modules. `module._extensions`, `module._pathCache`, `module._cache` are no-ops. `module.register` is not implemented; we recommend [instead.](/docs/runtime/plugins)

`Bun.plugin`
### [`node:net`](https://nodejs.org/api/net.html)

🟢 Fully implemented.
`node:net`
### [`node:perf_hooks`](https://nodejs.org/api/perf_hooks.html)

🟡 APIs are implemented, but the Node.js test suite for this module does not pass.
`node:perf_hooks`
### [`node:process`](https://nodejs.org/api/process.html)

🟡 See `node:process`
[Global.](#process)

`process`
### [`node:sys`](https://nodejs.org/api/util.html)

🟡 See `node:sys`
[.](#node-util)

`node:util`
### [`node:tls`](https://nodejs.org/api/tls.html)

🟡 Missing `node:tls``tls.createSecurePair`.
### [`node:util`](https://nodejs.org/api/util.html)

🟡 Missing `node:util``getCallSite` `getCallSites` `getSystemErrorMap` `getSystemErrorMessage` `transferableAbortSignal` `transferableAbortController`
### [`node:v8`](https://nodejs.org/api/v8.html)

🟡 `node:v8``writeHeapSnapshot` and `getHeapSnapshot` are implemented. `serialize` and `deserialize` use JavaScriptCore’s wire format instead of V8’s. Other methods are not implemented. For profiling, use [instead.](/docs/project/benchmarking#javascript-heap-stats)

`bun:jsc`
### [`node:vm`](https://nodejs.org/api/vm.html)

🟡 Core functionality and ES modules are implemented, including `node:vm``vm.Script`, `vm.createContext`, `vm.runInContext`, `vm.runInNewContext`, `vm.runInThisContext`, `vm.compileFunction`, `vm.isContext`, `vm.Module`, `vm.SourceTextModule`, `vm.SyntheticModule`, and `importModuleDynamically` support. Options like `timeout` and `breakOnSigint` are fully supported.
### [`node:wasi`](https://nodejs.org/api/wasi.html)

🟡 Partially implemented.
`node:wasi`
### [`node:worker_threads`](https://nodejs.org/api/worker_threads.html)

🟡 `node:worker_threads``Worker` doesn’t support the following options: `stdin` `stdout` `stderr` `trackedUnmanagedFds` `resourceLimits`. Missing `markAsUntransferable` `moveMessagePortToContext`.
### [`node:inspector`](https://nodejs.org/api/inspector.html)

🟡 Partially implemented. The `node:inspector``Profiler` API is supported (`Profiler.enable`, `Profiler.disable`, `Profiler.start`, `Profiler.stop`, `Profiler.setSamplingInterval`). Other inspector APIs are not implemented.
### [`node:repl`](https://nodejs.org/api/repl.html)

🟡 Mostly implemented. `node:repl``bun --interactive` starts a Node.js-compatible REPL. Result previews (which need V8’s inspector-based side-effect-free eval), tab-completion of `let`/`const`/`class` bindings in `useGlobal: true` mode, and some V8-specific error-message wording differ.
### [`node:sqlite`](https://nodejs.org/api/sqlite.html)

🟢 Fully implemented. `node:sqlite``backup()` runs synchronously and blocks the event loop for the duration of the copy (Node runs it on a worker thread). A `Buffer`/`Uint8Array` database path must be valid UTF-8 (Node passes the raw bytes through; Bun rejects non-UTF-8 with `ERR_INVALID_ARG_VALUE`). On macOS, Bun uses the system `libsqlite3.dylib`; `loadExtension()` (and, on older macOS releases, `createSession()`/`applyChangeset()`) require a full SQLite build — call `require("bun:sqlite").Database.setCustomSQLite(path)` before opening a database.
### [`node:test`](https://nodejs.org/api/test.html)

🟡 Partially implemented. The in-process API works when test files run under `node:test``bun test`: tests, suites, subtests, hooks, `t.plan()`, `t.assert`, `assert.register()`, `t.waitFor()`, `getTestContext()`, and `t.mock` (function/method/getter/setter/property mocks and mock timers). Missing `run()`, `node:test/reporters`, snapshot testing, `mock.module()`, code coverage, `--test-only`, test-level `signal`/`t.signal` abort, and Node’s `--test` CLI runner mode. `test.only()` / `{only: true}` are accepted but do not filter. `concurrency` is validated but subtests always run serially. Use [instead.](/docs/test)

`bun:test`
### [`node:trace_events`](https://nodejs.org/api/tracing.html)

🟢 Fully implemented.
`node:trace_events`
## Node.js globals

The following list covers every global implemented by Node.js and Bun’s compatibility status for each.
### [`AbortController`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)

🟢 Fully implemented.
`AbortController`
### [`AbortSignal`](https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal)

🟢 Fully implemented.
`AbortSignal`
### [`Blob`](https://developer.mozilla.org/en-US/docs/Web/API/Blob)

🟢 Fully implemented.
`Blob`
### [`Buffer`](https://nodejs.org/api/buffer.html#class-buffer)

🟢 Fully implemented.
`Buffer`
### [`ByteLengthQueuingStrategy`](https://developer.mozilla.org/en-US/docs/Web/API/ByteLengthQueuingStrategy)

🟢 Fully implemented.
`ByteLengthQueuingStrategy`
### [`__dirname`](https://nodejs.org/api/globals.html#__dirname)

🟢 Fully implemented.
`__dirname`
### [`__filename`](https://nodejs.org/api/globals.html#__filename)

🟢 Fully implemented.
`__filename`
### [`atob()`](https://developer.mozilla.org/en-US/docs/Web/API/atob)

🟢 Fully implemented.
`atob()`
### [`Atomics`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics)

🟢 Fully implemented.
`Atomics`
### [`BroadcastChannel`](https://developer.mozilla.org/en-US/docs/Web/API/BroadcastChannel)

🟢 Fully implemented.
`BroadcastChannel`
### [`btoa()`](https://developer.mozilla.org/en-US/docs/Web/API/btoa)

🟢 Fully implemented.
`btoa()`
### [`clearImmediate()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/clearImmediate)

🟢 Fully implemented.
`clearImmediate()`
### [`clearInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/clearInterval)

🟢 Fully implemented.
`clearInterval()`
### [`clearTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/clearTimeout)

🟢 Fully implemented.
`clearTimeout()`
### [`CompressionStream`](https://developer.mozilla.org/en-US/docs/Web/API/CompressionStream)

🟢 Fully implemented.
`CompressionStream`
### [`console`](https://developer.mozilla.org/en-US/docs/Web/API/console)

🟢 Fully implemented.
`console`
### [`CountQueuingStrategy`](https://developer.mozilla.org/en-US/docs/Web/API/CountQueuingStrategy)

🟢 Fully implemented.
`CountQueuingStrategy`
### [`Crypto`](https://developer.mozilla.org/en-US/docs/Web/API/Crypto)

🟢 Fully implemented.
`Crypto`
### [`SubtleCrypto (crypto)`](https://developer.mozilla.org/en-US/docs/Web/API/crypto)

🟢 Fully implemented.
`SubtleCrypto (crypto)`
### [`CryptoKey`](https://developer.mozilla.org/en-US/docs/Web/API/CryptoKey)

🟢 Fully implemented.
`CryptoKey`
### [`CustomEvent`](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent)

🟢 Fully implemented.
`CustomEvent`
### [`DecompressionStream`](https://developer.mozilla.org/en-US/docs/Web/API/DecompressionStream)

🟢 Fully implemented.
`DecompressionStream`
### [`Event`](https://developer.mozilla.org/en-US/docs/Web/API/Event)

🟢 Fully implemented.
`Event`
### [`EventTarget`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget)

🟢 Fully implemented.
`EventTarget`
### [`exports`](https://nodejs.org/api/globals.html#exports)

🟢 Fully implemented.
`exports`
### [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/fetch)

🟢 Fully implemented.
`fetch`
### [`FormData`](https://developer.mozilla.org/en-US/docs/Web/API/FormData)

🟢 Fully implemented.
`FormData`
### [`global`](https://nodejs.org/api/globals.html#global)

🟢 Implemented. `global``global` is an object containing all objects in the global namespace. It’s rarely referenced directly, as its contents are available without a prefix, for example `__dirname` instead of `global.__dirname`.
### [`globalThis`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis)

🟢 Aliases to `globalThis``global`.
### [`Headers`](https://developer.mozilla.org/en-US/docs/Web/API/Headers)

🟢 Fully implemented.
`Headers`
### [`MessageChannel`](https://developer.mozilla.org/en-US/docs/Web/API/MessageChannel)

🟢 Fully implemented.
`MessageChannel`
### [`MessageEvent`](https://developer.mozilla.org/en-US/docs/Web/API/MessageEvent)

🟢 Fully implemented.
`MessageEvent`
### [`MessagePort`](https://developer.mozilla.org/en-US/docs/Web/API/MessagePort)

🟢 Fully implemented.
`MessagePort`
### [`module`](https://nodejs.org/api/globals.html#module)

🟢 Fully implemented.
`module`
### [`PerformanceEntry`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceEntry)

🟢 Fully implemented.
`PerformanceEntry`
### [`PerformanceMark`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceMark)

🟢 Fully implemented.
`PerformanceMark`
### [`PerformanceMeasure`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceMeasure)

🟢 Fully implemented.
`PerformanceMeasure`
### [`PerformanceObserver`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceObserver)

🟢 Fully implemented.
`PerformanceObserver`
### [`PerformanceObserverEntryList`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceObserverEntryList)

🟢 Fully implemented.
`PerformanceObserverEntryList`
### [`PerformanceResourceTiming`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceResourceTiming)

🟢 Fully implemented.
`PerformanceResourceTiming`
### [`performance`](https://developer.mozilla.org/en-US/docs/Web/API/performance)

🟢 Fully implemented.
`performance`
### [`process`](https://nodejs.org/api/process.html)

🟡 Mostly implemented. `process``process.binding` (internal Node.js bindings some packages rely on) is partially implemented. `process.title` is a no-op on macOS & Linux. `getActiveResourcesInfo` `setActiveResourcesInfo`, `getActiveResources` and `setSourceMapsEnabled` are stubs. Newer APIs like `process.loadEnvFile` are not implemented.
### [`queueMicrotask()`](https://developer.mozilla.org/en-US/docs/Web/API/queueMicrotask)

🟢 Fully implemented.
`queueMicrotask()`
### [`ReadableByteStreamController`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableByteStreamController)

🟢 Fully implemented.
`ReadableByteStreamController`
### [`ReadableStream`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)

🟢 Fully implemented.
`ReadableStream`
### [`ReadableStreamBYOBReader`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamBYOBReader)

🟢 Fully implemented.
`ReadableStreamBYOBReader`
### [`ReadableStreamBYOBRequest`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamBYOBRequest)

🟢 Fully implemented.
`ReadableStreamBYOBRequest`
### [`ReadableStreamDefaultController`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamDefaultController)

🟢 Fully implemented.
`ReadableStreamDefaultController`
### [`ReadableStreamDefaultReader`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamDefaultReader)

🟢 Fully implemented.
`ReadableStreamDefaultReader`
### [`require()`](https://nodejs.org/api/globals.html#require)

🟢 Fully implemented, including `require()`
[,](https://nodejs.org/api/modules.html#requiremain)

`require.main`
[,](https://nodejs.org/api/modules.html#requirecache)

`require.cache`
[.](https://nodejs.org/api/modules.html#requireresolverequest-options)

`require.resolve`
### [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response)

🟢 Fully implemented.
`Response`
### [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request)

🟢 Fully implemented.
`Request`
### [`setImmediate()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setImmediate)

🟢 Fully implemented.
`setImmediate()`
### [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setInterval)

🟢 Fully implemented.
`setInterval()`
### [`setTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout)

🟢 Fully implemented.
`setTimeout()`
### [`structuredClone()`](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone)

🟢 Fully implemented.
`structuredClone()`
### [`SubtleCrypto`](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto)

🟢 Fully implemented.
`SubtleCrypto`
### [`DOMException`](https://developer.mozilla.org/en-US/docs/Web/API/DOMException)

🟢 Fully implemented.
`DOMException`
### [`TextDecoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder)

🟢 Fully implemented.
`TextDecoder`
### [`TextDecoderStream`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoderStream)

🟢 Fully implemented.
`TextDecoderStream`
### [`TextEncoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextEncoder)

🟢 Fully implemented.
`TextEncoder`
### [`TextEncoderStream`](https://developer.mozilla.org/en-US/docs/Web/API/TextEncoderStream)

🟢 Fully implemented.
`TextEncoderStream`
### [`TransformStream`](https://developer.mozilla.org/en-US/docs/Web/API/TransformStream)

🟢 Fully implemented.
`TransformStream`
### [`TransformStreamDefaultController`](https://developer.mozilla.org/en-US/docs/Web/API/TransformStreamDefaultController)

🟢 Fully implemented.
`TransformStreamDefaultController`
### [`URL`](https://developer.mozilla.org/en-US/docs/Web/API/URL)

🟢 Fully implemented.
`URL`
### [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)

🟢 Fully implemented.
`URLSearchParams`
### [`WebAssembly`](https://nodejs.org/api/globals.html#webassembly)

🟢 Fully implemented.
`WebAssembly`
### [`WritableStream`](https://developer.mozilla.org/en-US/docs/Web/API/WritableStream)

🟢 Fully implemented.
`WritableStream`
### [`WritableStreamDefaultController`](https://developer.mozilla.org/en-US/docs/Web/API/WritableStreamDefaultController)

🟢 Fully implemented.
`WritableStreamDefaultController`
### [`WritableStreamDefaultWriter`](https://developer.mozilla.org/en-US/docs/Web/API/WritableStreamDefaultWriter)

🟢 Fully implemented.`WritableStreamDefaultWriter`

# Citations

1. Source page: https://bun.sh/docs/runtime/nodejs-compat
