---
type: Web Page
title: Node.js Compatibility | Bun Docs
description: Bun's compatibility status with Node.js APIs, modules, and globals
resource: https://bun.sh/docs/runtime/nodejs-compat
timestamp: '2026-09-14T11:09:51.847401+00:00'
---

# Node.js Compatibility

Bun's compatibility status with Node.js APIs, modules, and globals

Every day, Bun gets closer to 100% Node.js API compatibility. Popular frameworks like Next.js, Express, and millions of `npm` packages intended for Node.js work with Bun. To ensure compatibility, we run thousands of tests from Node.js' test suite before every release of Bun.

**If a package works in Node.js but doesn't work in Bun, we consider it a bug in Bun.** [Open an issue](https://bun.com/issues) and we'll fix it.

We update this page regularly. It reflects the latest version of Bun's compatibility with *Node.js v26*.

## Built-in Node.js modules

### [`node:assert`](https://nodejs.org/api/assert.html)

`node:assert`
🟢 Fully implemented. Legacy-mode `deepEqual` uses `Bun.deepEquals` semantics rather than Node's loose `==` comparison, and function-valued or `printf`-style `message` arguments are not formatted.

### [`node:buffer`](https://nodejs.org/api/buffer.html)

`node:buffer`
🟢 Fully implemented. A single `Buffer` is capped at 4 GiB (`buffer.constants.MAX_LENGTH` is `2**32`).

### [`node:console`](https://nodejs.org/api/console.html)

`node:console`
🟢 Fully implemented. Bun writes console output directly to the stdout/stderr file descriptors and formats it with its own inspector. As a result, replacing `process.stdout.write` does not capture the output, and object layout differs from `util.inspect`. `console.trace()` writes to stdout and `console.time*()` to stderr.

### [`node:dgram`](https://nodejs.org/api/dgram.html)

`node:dgram`
🟢 Fully implemented. 99% of Node.js's test suite passes. `addMembership()` does not implicitly bind an unbound socket; call `bind()` first.

### [`node:diagnostics_channel`](https://nodejs.org/api/diagnostics_channel.html)

`node:diagnostics_channel`
🟡 `channel()`, `subscribe()`, `tracingChannel()` and the `http` client, `http2` and `dgram` built-in channels are implemented. Missing `boundedChannel()` and the `http.server.*`, `net`, `module`, `console`, `child_process` and `worker_threads` built-in channels. Subscribers do not keep a `Channel` alive, so hold a reference to it.

### [`node:dns`](https://nodejs.org/api/dns.html)

`node:dns`
🟢 Fully implemented. Missing `resolveTlsa`. Bun ignores the `Resolver` `maxTimeout` option.

### [`node:events`](https://nodejs.org/api/events.html)

`node:events`
🟢 Fully implemented. 95% of Node.js's test suite passes. `EventEmitterAsyncResource` uses `AsyncResource` underneath, so its `asyncId` is always `0`.

### [`node:fs`](https://nodejs.org/api/fs.html)

`node:fs`
🟢 Fully implemented. 98% of Node.js's test suite passes. `Stats` objects lack the `Temporal.Instant` getters (`atimeInstant` and friends).

### [`node:http`](https://nodejs.org/api/http.html)

`node:http`
🟢 Fully implemented. `http.Server` does not extend `net.Server`. Bun ignores `listen(handle)` and the `fd`, `ipv6Only` and `signal` options of `listen()`. `keepAlive`/`keepAliveInitialDelay` on the server are no-ops.

### [`node:https`](https://nodejs.org/api/https.html)

`node:https`
🟡 `request`, `get`, `Agent` and `globalAgent` are implemented, including connection pooling. Client sockets are `tls.TLSSocket`s. `https.Server` is `http.Server` with TLS options rather than a `tls.Server`. Its request sockets (`req.socket`) are not `tls.TLSSocket`s: `encrypted`, `authorized` and `servername` work, but `getPeerCertificate()` and `getCipher()` are missing. `setSecureContext()`, `addContext()`, `SNICallback` and `handshakeTimeout` are not supported.

### [`node:os`](https://nodejs.org/api/os.html)

`node:os`
🟢 Fully implemented. `userInfo()` reads `username`, `shell` and `homedir` from the environment (`USER`, `SHELL`, `HOME`) rather than the passwd database. `machine()` returns `"arm64"` instead of `"aarch64"` on Linux arm64.

### [`node:path`](https://nodejs.org/api/path.html)

`node:path`
🟢 Fully implemented. `matchesGlob()` uses `Bun.Glob` semantics rather than minimatch (`*` matches dotfiles, no extglobs). `path.win32` differs from Node in a few edge cases involving device paths and reserved names.

### [`node:punycode`](https://nodejs.org/api/punycode.html)

`node:punycode`
🟢 Fully implemented. 100% of Node.js's test suite passes. *Deprecated by Node.js*.

### [`node:querystring`](https://nodejs.org/api/querystring.html)

`node:querystring`
🟢 Fully implemented. 100% of Node.js's test suite passes.

### [`node:readline`](https://nodejs.org/api/readline.html)

`node:readline`
🟢 Fully implemented.

### [`node:stream`](https://nodejs.org/api/stream.html)

`node:stream`
🟢 Fully implemented. `isReadable`, `isWritable`, `isErrored` and `Readable.isDisturbed` only understand Node.js streams, not web streams.

### [`node:string_decoder`](https://nodejs.org/api/string_decoder.html)

`node:string_decoder`
🟢 Fully implemented. 100% of Node.js's test suite passes. `end()` does not accept a string argument.

### [`node:timers`](https://nodejs.org/api/timers.html)

`node:timers`
🟢 Fully implemented. The exports are the same functions as the globals. `node:timers/promises` (including `scheduler.wait()` and `scheduler.yield()`) is also implemented.

### [`node:tty`](https://nodejs.org/api/tty.html)

`node:tty`
🟢 Fully implemented. `ReadStream` and `WriteStream` extend the `fs` streams rather than `net.Socket`, and constructing them on a non-TTY fd returns a stream with `isTTY` set to `false` instead of throwing.

### [`node:url`](https://nodejs.org/api/url.html)

`node:url`
🟢 Fully implemented.

### [`node:zlib`](https://nodejs.org/api/zlib.html)

`node:zlib`
🟢 Fully implemented. 98% of Node.js's test suite passes.

### [`node:async_hooks`](https://nodejs.org/api/async_hooks.html)

`node:async_hooks`
🟡 `AsyncLocalStorage` and `AsyncResource` are implemented. `createHook`, `executionAsyncId`, `triggerAsyncId` and `executionAsyncResource` are stubs: Bun does not invoke hooks, apart from `init` for `process.nextTick`, and async ids are always `0`. Node.js [strongly discourages](https://nodejs.org/docs/latest/api/async_hooks.html#async-hooks) these APIs in favor of `AsyncLocalStorage`. Bun does not propagate `AsyncLocalStorage` context into `MessagePort`, `BroadcastChannel` or `Worker` events.

### [`node:child_process`](https://nodejs.org/api/child_process.html)

`node:child_process`
🟡 IPC can send `net.Socket`, `net.Server` and `dgram.Socket` handles (including to and from Node.js processes), but not `http` server sockets. `serialization: "advanced"` only works between Bun processes, so use JSON serialization for Node.js ↔ Bun IPC. Missing `subprocess.channel.ref()`/`unref()`. You cannot pass a child's `stdout`/`stderr` as another child's `stdio`, and `spawnSync` does not return extra `stdio` pipes in `output`.

### [`node:cluster`](https://nodejs.org/api/cluster.html)

`node:cluster`
🟡 `net` and `dgram` servers in workers are shared through the primary as in Node.js (`SCHED_RR` and `SCHED_NONE`), and handles can be passed with `worker.send()`. `node:http`/`node:https` servers in workers each bind their own socket instead, so load-balancing HTTP requests across processes is only supported on Linux (through `SO_REUSEPORT`). Otherwise, implemented but not battle-tested.

### [`node:crypto`](https://nodejs.org/api/crypto.html)

`node:crypto`
🟡 Missing `encapsulate`/`decapsulate` (you can use ML-KEM keys through `crypto.subtle`). `argon2()` and `argon2Sync()` are implemented. Custom engines (`setEngine()`) throw, `setFips()` is a no-op and `secureHeapUsed()` returns `undefined`. Bun's crypto is backed by BoringSSL, which lacks the `ed448`, `x448`, `rsa-pss`, `dsa`, `dh` and `ml-kem-512` key types, EC curves other than P-224/256/384/521 (no `secp256k1`), and the CCM, OCB, XTS and `chacha20-poly1305` ciphers. The `DiffieHellman` class (`createDiffieHellman()`, `getDiffieHellman()`) works.

### [`node:domain`](https://nodejs.org/api/domain.html)

`node:domain`
🟡 Missing `Domain` `members`. A domain only catches errors thrown synchronously inside `run()`/`bind()` or emitted by emitters passed to `add()`. Bun does not route errors from timers, `process.nextTick`, promises and other async callbacks to the domain.

### [`node:http2`](https://nodejs.org/api/http2.html)

`node:http2`
🟢 Client & server are implemented. 94% of Node.js's test suite passes. The `maxDeflateDynamicTableSize`, `peerMaxConcurrentStreams` and `streamResetBurst`/`streamResetRate` options are accepted but ignored.

### [`node:module`](https://nodejs.org/api/module.html)

`node:module`
🟡 Missing `Module#load()`, `registerHooks`, `findPackageJSON`, `stripTypeScriptTypes`, `getSourceMapsSupport`/`setSourceMapsSupport`. Overriding `require.cache`, `require.extensions` and `module._resolveFilename` is supported. `syncBuiltinESMExports`, `module._load`, `module._pathCache` and `module.register` are no-ops (we recommend [`Bun.plugin`](/docs/runtime/plugins) instead). `findSourceMap` always returns `undefined`.

### [`node:net`](https://nodejs.org/api/net.html)

`node:net`
🟢 Fully implemented, including `BlockList`, `SocketAddress`, `autoSelectFamily`, Unix domain sockets and `server.listen({ fd })`. `new net.Socket({ fd })` cannot read from an existing file descriptor (only write-only wrapping works). `server.listen(handle)` only accepts `{ fd }`. Missing `blockList.toJSON()`/`fromJSON()`.

### [`node:perf_hooks`](https://nodejs.org/api/perf_hooks.html)

`node:perf_hooks`
🟡 `monitorEventLoopDelay()`, `createHistogram()`, `timerify()` and `PerformanceObserver` (`mark`, `measure`, `function`, `net`, `http` and `http2` entries) are implemented. Bun never emits `gc`, `dns` or `resource` entries. `eventLoopUtilization()` always returns zeros, and `performance.nodeTiming` holds placeholder values. The Node-specific additions to the global `performance` object only appear once `node:perf_hooks` has been imported.

### [`node:process`](https://nodejs.org/api/process.html)

`node:process`
🟡 See [`process`](#process) Global.

### [`node:sys`](https://nodejs.org/api/util.html)

`node:sys`
🟢 See [`node:util`](#node-util).

### [`node:tls`](https://nodejs.org/api/tls.html)

`node:tls`
🟡 Missing `pskCallback`, OCSP stapling (`requestOCSP`), the server `'newSession'`/`'resumeSession'` events and session ticket keys (`ticketKeys` is ignored). As a result, session resumption does not work across processes. Bun uses BoringSSL, so `tlsSocket.renegotiate()` always fails and `getEphemeralKeyInfo()`/`getSharedSigalgs()` return no information.

### [`node:util`](https://nodejs.org/api/util.html)

`node:util`
🟢 Fully implemented. Missing `diff` (experimental in Node.js), `transferableAbortSignal` and `transferableAbortController`. `debuglog()` ignores its `callback` argument and the returned function has no `enabled` property.

### [`node:v8`](https://nodejs.org/api/v8.html)

`node:v8`
🟡 `writeHeapSnapshot`, `getHeapSnapshot`, `getHeapStatistics`, `getHeapSpaceStatistics`, `GCProfiler` and `startupSnapshot` are implemented. The heap statistics describe JavaScriptCore's single heap, and `setFlagsFromString` ignores the flags it is given. `serialize` and `deserialize` use JavaScriptCore's wire format instead of V8's. Missing `queryObjects`, `startCpuProfile`, `startHeapProfile`, `Serializer`/`Deserializer`, `takeCoverage`/`stopCoverage` and `promiseHooks`. For profiling, use [`bun:jsc`](/docs/project/benchmarking#javascript-heap-stats) instead.

### [`node:vm`](https://nodejs.org/api/vm.html)

`node:vm`
🟢 Fully implemented, including `vm.Script`, `vm.createContext`, `vm.runInContext`, `vm.runInNewContext`, `vm.runInThisContext`, `vm.compileFunction`, `vm.isContext`, the ES module classes `vm.Module`, `vm.SourceTextModule` and `vm.SyntheticModule` (exported without `--experimental-vm-modules`), and `importModuleDynamically` support. The `timeout`, `breakOnSigint`, `cachedData`, `microtaskMode` and `codeGeneration` options are supported. An `importModuleDynamically` callback that returns a promise for a `vm.Module` resolves `import()` to the module object rather than its namespace. `vm.measureMemory()` reports whole-heap figures for every context.

### [`node:wasi`](https://nodejs.org/api/wasi.html)

`node:wasi`
🟡 Partially implemented. `WASI` supports `args`, `env`, `preopens`, `wasiImport` and `start()`, and `bun ./program.wasm` runs a WASI command directly. Missing `getImportObject()` (use `wasiImport`), `initialize()` and the `sock_accept` import. Bun ignores the `version`, `returnOnExit`, `stdin`, `stdout` and `stderr` options, so `proc_exit` exits the Bun process.

### [`node:worker_threads`](https://nodejs.org/api/worker_threads.html)

`node:worker_threads`
🟡 `Worker` ignores the `resourceLimits` and `trackUnmanagedFds` options. Some `execArgv` flags take effect in the worker, for example `--no-addons`, `--stack-trace-limit` and `--tls-min-v1.3`. Others, for example `--conditions` and `--no-deprecation`, only set `process.execArgv`. `worker.performance.eventLoopUtilization()` is a stub. Missing `moveMessagePortToContext` and `locks`.

### [`node:inspector`](https://nodejs.org/api/inspector.html)

`node:inspector`
🟡 Partially implemented. `Session` supports the `Profiler` domain (including precise coverage), `Runtime.enable` and `NodeTracing`, from both `node:inspector` and `node:inspector/promises`. After `open()`, `Session` also forwards `Debugger` configuration commands such as `Debugger.enable` and `Debugger.setBreakpointByUrl` to the inspector server. Their results, such as `breakpointId`, are not returned. Other `Session` commands such as `Runtime.evaluate` and the `HeapProfiler` domain are not implemented. `open()`, `url()`, `close()` and `waitForDebugger()` are implemented. `open()` serves the `Debugger` and `Runtime` domains and throws in workers. Missing `Network`.

### [`node:repl`](https://nodejs.org/api/repl.html)

`node:repl`
🟡 Mostly implemented. `bun --interactive` starts a Node.js-compatible REPL. The REPL does not show result previews (they need V8's inspector-based side-effect-free eval). Tab-completion skips `let`/`const`/`class` bindings, and some V8-specific error-message and stack-frame wording differs.

### [`node:sqlite`](https://nodejs.org/api/sqlite.html)

`node:sqlite`
🟢 Fully implemented. `backup()` runs synchronously and blocks the event loop for the duration of the copy (Node runs it on a worker thread). A `Buffer`/`Uint8Array` database path must be valid UTF-8 (Node passes the raw bytes through; Bun rejects non-UTF-8 with `ERR_INVALID_ARG_VALUE`). On macOS, Bun uses the system `libsqlite3.dylib`. `loadExtension()` requires a full SQLite build, and so do `createSession()`/`applyChangeset()` on older macOS releases. To use a full SQLite build, call `require("bun:sqlite").Database.setCustomSQLite(path)` before opening a database.

### [`node:test`](https://nodejs.org/api/test.html)

`node:test`
🟡 Partially implemented. The in-process API works when test files run under `bun test`: tests, suites, subtests, hooks, `t.plan()`, `t.assert`, `assert.register()`, `t.waitFor()`, `getTestContext()`, `expectFailure`, and `t.mock` (function/method/getter/setter/property mocks and mock timers). `run()` requires an explicit `files` list and runs each file in a `bun test` child process. Most of its options (`globPatterns`, `watch`, `coverage`, `shard`, `only`, `testNamePatterns`, ...) throw `ERR_NOT_IMPLEMENTED`. Missing `node:test/reporters`, snapshot testing, `mock.module()`, `t.runOnly()`, code coverage, `--test-only`, test-level `signal` abort, and Node's `--test` CLI runner mode. `test.only()` / `{only: true}` are accepted but do not filter. `concurrency` is validated but subtests always run serially. Use [`bun:test`](/docs/test) instead.

### [`node:trace_events`](https://nodejs.org/api/tracing.html)

`node:trace_events`
🟢 Fully implemented. `createTracing()`, `getEnabledCategories()` and the `--trace-events-enabled`, `--trace-event-categories` and `--trace-event-file-pattern` flags are supported. Bun writes the trace at exit. Some categories record less than in Node.js. For example, `node.async_hooks` only records timers, and the `v8` category is a placeholder, since JavaScriptCore has no V8 GC or compile events.

### [`node:quic`](https://github.com/nodejs/node/blob/main/doc/api/quic.md)

`node:quic`
🟢 Implemented: `listen()`, `connect()`, `QuicEndpoint`, `QuicSession` and `QuicStream`. 99% of Node.js's test suite passes. The API is experimental in Node.js, and importing it emits an `ExperimentalWarning` in Bun too.

### [`node:sea`](https://nodejs.org/api/single-executable-applications.html)

`node:sea`
🔴 Not implemented. Use [`bun build --compile`](/docs/bundler/executables) to build single-file executables instead.

## Node.js globals

The following list covers the globals implemented by Node.js and Bun's compatibility status for each.

### [`AbortController`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)

`AbortController`
🟢 Fully implemented.

### [`AbortSignal`](https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal)

`AbortSignal`
🟢 Fully implemented.

### [`Blob`](https://developer.mozilla.org/en-US/docs/Web/API/Blob)

`Blob`
🟢 Fully implemented. The `endings` constructor option is ignored, and `blob.stream()` does not support BYOB readers.

### [`Buffer`](https://nodejs.org/api/buffer.html#class-buffer)

`Buffer`
🟢 Fully implemented. A single `Buffer` is capped at 4 GiB (`buffer.constants.MAX_LENGTH` is `2**32`).

### [`ByteLengthQueuingStrategy`](https://developer.mozilla.org/en-US/docs/Web/API/ByteLengthQueuingStrategy)

`ByteLengthQueuingStrategy`
🟢 Fully implemented.

### [`__dirname`](https://nodejs.org/api/globals.html#__dirname)

`__dirname`
🟢 Fully implemented.

### [`__filename`](https://nodejs.org/api/globals.html#__filename)

`__filename`
🟢 Fully implemented.

### [`atob()`](https://developer.mozilla.org/en-US/docs/Web/API/atob)

`atob()`
🟢 Fully implemented.

### [`Atomics`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics)

`Atomics`
🟢 Fully implemented.

### [`BroadcastChannel`](https://developer.mozilla.org/en-US/docs/Web/API/BroadcastChannel)

`BroadcastChannel`
🟢 Fully implemented.

### [`btoa()`](https://developer.mozilla.org/en-US/docs/Web/API/btoa)

`btoa()`
🟢 Fully implemented.

### [`clearImmediate()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/clearImmediate)

`clearImmediate()`
🟢 Fully implemented.

### [`clearInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/clearInterval)

`clearInterval()`
🟢 Fully implemented.

### [`clearTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/clearTimeout)

`clearTimeout()`
🟢 Fully implemented.

### [`CloseEvent`](https://developer.mozilla.org/en-US/docs/Web/API/CloseEvent)

`CloseEvent`
🟢 Fully implemented.

### [`CompressionStream`](https://developer.mozilla.org/en-US/docs/Web/API/CompressionStream)

`CompressionStream`
🟢 Fully implemented.

### [`console`](https://developer.mozilla.org/en-US/docs/Web/API/console)

`console`
🟢 Fully implemented. See [`node:console`](#node-console) for the differences in how output is written.

### [`CountQueuingStrategy`](https://developer.mozilla.org/en-US/docs/Web/API/CountQueuingStrategy)

`CountQueuingStrategy`
🟢 Fully implemented.

### [`Crypto`](https://developer.mozilla.org/en-US/docs/Web/API/Crypto)

`Crypto`
🟢 Fully implemented.

### [`SubtleCrypto (crypto)`](https://developer.mozilla.org/en-US/docs/Web/API/crypto)

`SubtleCrypto (crypto)`
🟢 Fully implemented. See [`SubtleCrypto`](#subtlecrypto) for the algorithms Bun does not support.

### [`CryptoKey`](https://developer.mozilla.org/en-US/docs/Web/API/CryptoKey)

`CryptoKey`
🟢 Fully implemented.

### [`CustomEvent`](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent)

`CustomEvent`
🟢 Fully implemented.

### [`DecompressionStream`](https://developer.mozilla.org/en-US/docs/Web/API/DecompressionStream)

`DecompressionStream`
🟢 Fully implemented.

### [`ErrorEvent`](https://developer.mozilla.org/en-US/docs/Web/API/ErrorEvent)

`ErrorEvent`
🟢 Fully implemented.

### [`Event`](https://developer.mozilla.org/en-US/docs/Web/API/Event)

`Event`
🟢 Fully implemented.

### [`EventTarget`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget)

`EventTarget`
🟢 Fully implemented.

### [`exports`](https://nodejs.org/api/globals.html#exports)

`exports`
🟢 Fully implemented.

### [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/fetch)

`fetch`
🟢 Fully implemented. The `integrity` option is ignored.

### [`File`](https://developer.mozilla.org/en-US/docs/Web/API/File)

`File`
🟢 Fully implemented. `File` objects report `Blob` as their `constructor` and `Symbol.toStringTag`.

### [`FormData`](https://developer.mozilla.org/en-US/docs/Web/API/FormData)

`FormData`
🟢 Fully implemented.

### [`global`](https://nodejs.org/api/globals.html#global)

`global`
🟢 Implemented. `global` is an object containing all objects in the global namespace. It's rarely referenced directly, as its contents are available without a prefix, for example `console` instead of `global.console`.

### [`globalThis`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis)

`globalThis`
🟢 Aliases to `global`.

### [`Headers`](https://developer.mozilla.org/en-US/docs/Web/API/Headers)

`Headers`
🟢 Fully implemented.

### [`MessageChannel`](https://developer.mozilla.org/en-US/docs/Web/API/MessageChannel)

`MessageChannel`
🟢 Fully implemented.

### [`MessageEvent`](https://developer.mozilla.org/en-US/docs/Web/API/MessageEvent)

`MessageEvent`
🟢 Fully implemented.

### [`MessagePort`](https://developer.mozilla.org/en-US/docs/Web/API/MessagePort)

`MessagePort`
🟢 Fully implemented. The EventEmitter-style methods Node.js adds (`on()`, `once()`, `off()`, ...) are only installed once `node:worker_threads` has been loaded.

### [`module`](https://nodejs.org/api/globals.html#module)

`module`
🟢 Fully implemented. Missing `module.isPreloading`.

### [`navigator`](https://nodejs.org/api/globals.html#navigator)

`navigator`
🟡 `userAgent`, `platform` and `hardwareConcurrency` are implemented. Missing `language`, `languages` and `locks`. The `Navigator` class is not a global.

### [`PerformanceEntry`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceEntry)

`PerformanceEntry`
🟢 Fully implemented.

### [`PerformanceMark`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceMark)

`PerformanceMark`
🟢 Fully implemented.

### [`PerformanceMeasure`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceMeasure)

`PerformanceMeasure`
🟢 Fully implemented.

### [`PerformanceObserver`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceObserver)

`PerformanceObserver`
🟡 Observing `mark` and `measure` entries works. Bun only delivers Node-only entry types (`function`, `http`, `net`, ...) to the `node:perf_hooks` `PerformanceObserver`, and never emits `gc`, `dns` or `resource` entries.

### [`PerformanceObserverEntryList`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceObserverEntryList)

`PerformanceObserverEntryList`
🟢 Fully implemented.

### [`PerformanceResourceTiming`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceResourceTiming)

`PerformanceResourceTiming`
🟡 The class exists, but no entries are ever created: `fetch()` does not record resource timing and `performance.markResourceTiming()` is a no-op.

### [`performance`](https://developer.mozilla.org/en-US/docs/Web/API/performance)

`performance`
🟡 `now()`, `timeOrigin`, `mark()`, `measure()` and `getEntries()` are implemented. The Node.js additions (`eventLoopUtilization()`, `nodeTiming`, `timerify()`) only exist once `node:perf_hooks` has been loaded. `eventLoopUtilization()` always returns zeros and `nodeTiming` holds placeholder values.

### [`process`](https://nodejs.org/api/process.html)

`process`
🟡 Mostly implemented. `process.binding` (internal Node.js bindings some packages rely on) is partially implemented: `buffer`, `config`, `constants`, `crypto/x509`, `fs`, `http_parser`, `natives`, `tty_wrap`, `util` and `uv` are available, the rest throw. Setting `process.title` is a no-op on macOS & Linux. `getActiveResourcesInfo()`, `_getActiveHandles()` and `_getActiveRequests()` always return an empty array, `setSourceMapsEnabled()` is a no-op, and `process.report.writeReport()` writes nothing. Missing `sourceMapsEnabled` and `addUncaughtExceptionCaptureCallback`.

### [`queueMicrotask()`](https://developer.mozilla.org/en-US/docs/Web/API/queueMicrotask)

`queueMicrotask()`
🟢 Fully implemented.

### [`QuotaExceededError`](https://developer.mozilla.org/en-US/docs/Web/API/QuotaExceededError)

`QuotaExceededError`
🔴 Not implemented.

### [`ReadableByteStreamController`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableByteStreamController)

`ReadableByteStreamController`
🟢 Fully implemented.

### [`ReadableStream`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)

`ReadableStream`
🟢 Fully implemented. Streams cannot be transferred with `postMessage()` or `structuredClone()`.

### [`ReadableStreamBYOBReader`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamBYOBReader)

`ReadableStreamBYOBReader`
🟢 Fully implemented.

### [`ReadableStreamBYOBRequest`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamBYOBRequest)

`ReadableStreamBYOBRequest`
🟢 Fully implemented.

### [`ReadableStreamDefaultController`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamDefaultController)

`ReadableStreamDefaultController`
🟢 Fully implemented.

### [`ReadableStreamDefaultReader`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamDefaultReader)

`ReadableStreamDefaultReader`
🟢 Fully implemented.

### [`require()`](https://nodejs.org/api/globals.html#require)

`require()`
🟢 Fully implemented, including [`require.main`](https://nodejs.org/api/modules.html#requiremain), [`require.cache`](https://nodejs.org/api/modules.html#requirecache), [`require.resolve`](https://nodejs.org/api/modules.html#requireresolverequest-options).

### [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response)

`Response`
🟢 Fully implemented. A `Response` constructed from a string does not expose the default `content-type` header in `headers` (`Bun.serve()` still sends it).

### [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request)

`Request`
🟡 Missing `keepalive` and `duplex`. The `credentials`, `integrity`, `referrer` and `referrerPolicy` options are accepted but ignored.

### [`setImmediate()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setImmediate)

`setImmediate()`
🟢 Fully implemented.

### [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setInterval)

`setInterval()`
🟢 Fully implemented.

### [`setTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout)

`setTimeout()`
🟢 Fully implemented.

### [`structuredClone()`](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone)

`structuredClone()`
🟢 Fully implemented. Only `ArrayBuffer` and `MessagePort` can be transferred, and cloned `Error`s lose their `cause`.

### [`Storage`](https://developer.mozilla.org/en-US/docs/Web/API/Storage)

`Storage`
🔴 Not implemented. Bun has no `Storage`, `localStorage` or `sessionStorage` globals.

### [`SubtleCrypto`](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto)

`SubtleCrypto`
🟢 Fully implemented, including `supports()`, `getPublicKey()`, the `encapsulate*()`/`decapsulate*()` methods, `ML-DSA`, `ML-KEM-768`/`ML-KEM-1024`, `SHA3-*` and `ChaCha20-Poly1305`. Missing the `Ed448`, `X448`, `AES-OCB`, `Argon2*`, `cSHAKE*`, `KMAC*`, `KT128`/`KT256`, `TurboSHAKE*` and `ML-KEM-512` algorithms (all experimental in Node.js).

### [`DOMException`](https://developer.mozilla.org/en-US/docs/Web/API/DOMException)

`DOMException`
🟢 Fully implemented. Instances are not native errors (`Error.isError()` returns `false`).

### [`TextDecoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder)

`TextDecoder`
🟢 Fully implemented.

### [`TextDecoderStream`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoderStream)

`TextDecoderStream`
🟢 Fully implemented.

### [`TextEncoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextEncoder)

`TextEncoder`
🟢 Fully implemented.

### [`TextEncoderStream`](https://developer.mozilla.org/en-US/docs/Web/API/TextEncoderStream)

`TextEncoderStream`
🟢 Fully implemented.

### [`TransformStream`](https://developer.mozilla.org/en-US/docs/Web/API/TransformStream)

`TransformStream`
🟢 Fully implemented. Cannot be transferred with `postMessage()` or `structuredClone()`.

### [`TransformStreamDefaultController`](https://developer.mozilla.org/en-US/docs/Web/API/TransformStreamDefaultController)

`TransformStreamDefaultController`
🟢 Fully implemented.

### [`URL`](https://developer.mozilla.org/en-US/docs/Web/API/URL)

`URL`
🟢 Fully implemented.

### [`URLPattern`](https://developer.mozilla.org/en-US/docs/Web/API/URLPattern)

`URLPattern`
🟢 Fully implemented.

### [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)

`URLSearchParams`
🟢 Fully implemented.

### [`WebAssembly`](https://nodejs.org/api/globals.html#webassembly)

`WebAssembly`
🟢 Fully implemented. Memory64 is disabled by default (set `BUN_JSC_useWasmMemory64=1` to enable it).

### [`WebSocket`](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

`WebSocket`
🟢 Fully implemented. `binaryType` defaults to `"nodebuffer"`, so binary messages arrive as `Buffer`s. Node.js defaults to `"blob"`.

### [`WritableStream`](https://developer.mozilla.org/en-US/docs/Web/API/WritableStream)

`WritableStream`
🟢 Fully implemented. Cannot be transferred with `postMessage()` or `structuredClone()`.

### [`WritableStreamDefaultController`](https://developer.mozilla.org/en-US/docs/Web/API/WritableStreamDefaultController)

`WritableStreamDefaultController`
🟢 Fully implemented.

### [`WritableStreamDefaultWriter`](https://developer.mozilla.org/en-US/docs/Web/API/WritableStreamDefaultWriter)

`WritableStreamDefaultWriter`
🟢 Fully implemented.

# Citations

1. Source page: https://bun.sh/docs/runtime/nodejs-compat
