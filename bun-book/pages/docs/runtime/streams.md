---
type: Web Page
title: Streams - Bun
description: Use Bun's streams API to work with binary data without loading it all
  into memory at once
resource: https://bun.sh/docs/runtime/streams
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

[and](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)

`ReadableStream`[.](https://developer.mozilla.org/en-US/docs/Web/API/WritableStream)

`WritableStream`Bun also implements the 

`node:stream` module, including
[,](https://nodejs.org/api/stream.html#stream_readable_streams)`Readable`[, and](https://nodejs.org/api/stream.html#stream_writable_streams)`Writable`[. For complete documentation, refer to the](https://nodejs.org/api/stream.html#stream_duplex_and_transform_streams)`Duplex`[Node.js docs](https://nodejs.org/api/stream.html).`ReadableStream`:
`ReadableStream` can be read chunk-by-chunk with `for await` syntax.
## Direct `ReadableStream`

Bun implements an optimized version of `ReadableStream` that avoids unnecessary data copying and queue management.
With a traditional `ReadableStream`, chunks of data are *enqueued*. Each chunk is copied into a queue, where it sits until the stream is ready to send more data.

`ReadableStream`, chunks of data are written directly to the stream. No queueing happens, and there’s no need to clone the chunk data into memory. The `controller` API reflects this: you call `.write()` instead of `.enqueue()`.
`ReadableStream`, the destination handles all chunk queueing. The consumer of the stream receives exactly what is passed to `controller.write()`, without any encoding or modification.
### Handling backpressure

`controller.write()` returns the number of bytes written. When the destination’s internal buffer is full (for example, a slow HTTP client), it returns a **negative number**instead. The chunk is still accepted — the negative return is a signal to pause and wait for the destination to drain. To wait for the drain,

`await controller.flush(true)`:
`direct`) `ReadableStream`s and async-generator response bodies, Bun applies this backpressure automatically — the producer is paused while the destination is backed up.
## Async generator streams

Bun also supports async generator functions as a source for`Response` and `Request`. Use async generators to create a `ReadableStream` that fetches data from an asynchronous source.
`[Symbol.asyncIterator]` directly.
`yield` returns the direct `ReadableStream` controller.
`Bun.ArrayBufferSink`

The `Bun.ArrayBufferSink` class is a fast incremental writer for constructing an `ArrayBuffer` of unknown size.
`Uint8Array`, pass the `asUint8Array` option to the `start` method.
`.write()` method supports strings, typed arrays, `ArrayBuffer`, and `SharedArrayBuffer`.
`.end()` is called, no more data can be written to the `ArrayBufferSink`. However, when buffering a stream you may want to keep writing data and periodically `.flush()` the contents (say, into a `WritableStream`). To support this, pass `stream: true` to the `start` method.
`.flush()` method returns the buffered data as an `ArrayBuffer` (or `Uint8Array` if `asUint8Array: true`) and clears the internal buffer.
To manually set the size of the internal buffer in bytes, pass a value for `highWaterMark`:
## Reference

See Typescript Definitions

# Citations

1. Source page: https://bun.sh/docs/runtime/streams
