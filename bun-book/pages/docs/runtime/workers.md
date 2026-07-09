---
type: Web Page
title: Workers - Bun
description: Use Bun's Workers API to create and communicate with a new JavaScript
  instance running on a separate thread while sharing I/O resources with the main
  thread
resource: https://bun.sh/docs/runtime/workers
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

[, you start and communicate with a new JavaScript instance running on a separate thread while sharing I/O resources with the main thread. Bun implements a minimal version of the](https://developer.mozilla.org/en-US/docs/Web/API/Worker)

`Worker`[Web Workers API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API)with extensions that make it work better for server-side use cases. Like the rest of Bun,

`Worker` supports CommonJS, ES modules, TypeScript, JSX, and TSX with no extra build step.
## Creating a `Worker`

Like in browsers, [is a global. Use it to create a new worker thread.](https://developer.mozilla.org/en-US/docs/Web/API/Worker)

`Worker`### From the main thread

index.ts

### Worker thread

worker.ts

`self`, add this line to the top of your worker file.
`import` and `export` syntax in your worker code. Unlike in browsers, you don’t need to pass `{type: "module"}` to use ES modules.
If the worker’s script fails to resolve, an `"error"` event is emitted on the `Worker` object.
`Worker` is resolved relative to the project root (like typing `bun ./path/to/file.js`).
`preload` - load modules before the worker starts

Pass an array of module specifiers to the `preload` option to load them before the worker’s own code runs, like the `--preload` CLI argument. Use it for code that must load first, such as OpenTelemetry, Sentry, or DataDog.
index.ts

`preload` option:
index.ts

`blob:` URLs

You can also pass a `blob:` URL to `Worker` to create a worker from a string or other in-memory source.
`blob:` URLs support TypeScript, JSX, and other file types. To tell Bun the source is TypeScript, set the `type` on the `Blob` or pass a `filename` to the `File` constructor.
`"open"`

The `"open"` event is emitted when a worker is created and ready to receive messages. (This event does not exist in browsers.)
index.ts

`"open"` event before sending.
## Messages with `postMessage`

To send messages, use [and](https://developer.mozilla.org/en-US/docs/Web/API/Worker/postMessage)

`worker.postMessage`[. Messages are serialized with the](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage)

`self.postMessage`[HTML Structured Clone Algorithm](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm).

### Performance optimizations

Bun has fast paths for`postMessage` with common data types:
**String fast path**- When posting a pure string, Bun bypasses the structured clone algorithm entirely, so there is no serialization overhead.

**Simple object fast path**- For plain objects containing only primitive values (strings, numbers, booleans, null, undefined), Bun stores properties directly without full structured cloning. The simple object fast path activates when the object:

- Is a plain object with no prototype chain modifications
- Contains only enumerable, configurable data properties
- Has no indexed properties or getter/setter methods
- All property values are primitives or strings

`postMessage` performs **2-241x faster**because the message length no longer has a meaningful impact on performance.

**Bun (with fast paths):**

**Node.js v24.6.0 (for comparison):**

[on the worker and main thread.](https://developer.mozilla.org/en-US/docs/Web/API/Worker/message_event)

`message` event handler## Terminating a worker

A`Worker` instance terminates automatically once its event loop has no work left to do. Attaching a `"message"` listener on the global or any `MessagePort`s keeps the event loop alive. To forcefully terminate a `Worker`, call `worker.terminate()`.
index.ts

`worker.terminate()` makes the worker exit as soon as possible.
`process.exit()`

A worker can terminate itself with `process.exit()`. This does not terminate the main process. Like in Node.js, `process.on('beforeExit', callback)` and `process.on('exit', callback)` are emitted on the worker thread (and not on the main thread), and the exit code is passed to the `"close"` event.
`"close"`

The `"close"` event is emitted when a worker has been marked as terminated; the worker itself can take some time to fully exit. The `CloseEvent` contains the exit code passed to `process.exit()`, or 0 if it closed for another reason.
index.ts

## Managing lifetime

By default, an active`Worker` keeps the main (spawning) process alive, so async tasks like `setTimeout` and promises keep the process alive. Attaching `message` listeners also keeps the `Worker` alive.
`worker.unref()`

To stop a running worker from keeping the process alive, call `worker.unref()`. This decouples the worker’s lifetime from the main process’s, matching the behavior of Node.js’ `worker_threads`.
index.ts

`worker.unref()` is not available in browsers.
`worker.ref()`

To keep the process alive until the `Worker` terminates, call `worker.ref()`. Workers are ref’d by default; a ref’d worker still needs something on its event loop (such as a `"message"` listener) to continue running.
index.ts

`options` object to `Worker`:
index.ts

`worker.ref()` is not available in browsers.
## Memory usage with `smol`

Bun’s `Worker` supports a `smol` mode that reduces memory usage at a cost of performance. To enable it, pass `smol: true` in the `Worker` constructor’s `options` object.
index.ts

What does `smol` mode actually do?

What does `smol` mode actually do?

Setting 

`smol: true` sets `JSC::HeapSize` to be `Small` instead of the default `Large`.## Environment Data

Share data between the main thread and workers using`setEnvironmentData()` and `getEnvironmentData()`.
index.ts

## Worker Events

Listen for worker creation events using`process.on()`:
index.ts

`Bun.isMainThread`

Check `Bun.isMainThread` to tell whether you’re on the main thread.

# Citations

1. Source page: https://bun.sh/docs/runtime/workers
