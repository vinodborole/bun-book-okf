---
type: Web Page
title: Utils - Bun
description: Use Bun's utility functions to work with the runtime
resource: https://bun.sh/docs/runtime/utils
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`Bun.version`

A `string` containing the version of the `bun` CLI that is currently running.
terminal

`Bun.revision`

The git commit of Bun that was compiled to create the current `bun` CLI.
terminal

`Bun.env`

An alias for `process.env`.
`Bun.main`

An absolute path to the entrypoint of the current program (the file that was executed with `bun run`).
script.ts

`require.main = module` trick in Node.js.
`Bun.sleep()`

`Bun.sleep(ms: number)`
Returns a `Promise` that resolves after the given number of milliseconds.
`Date` object to receive a `Promise` that resolves at that point in time.
`Bun.sleepSync()`

`Bun.sleepSync(ms: number)`
A blocking synchronous version of `Bun.sleep`.
`Bun.which()`

`Bun.which(bin: string)`
Returns the path to an executable, similar to typing `which` in your terminal.
`PATH` environment variable to determine the path. To configure `PATH`:
`cwd` option to resolve the executable from within a specific directory.
`which` npm package.
`Bun.randomUUIDv7()`

`Bun.randomUUIDv7()` returns a UUID v7, which is monotonic and suitable for sorting and databases.
`timestamp` parameter defaults to the current time in milliseconds. When the timestamp changes, the counter is reset to a pseudo-random integer wrapped to 4096. This counter is atomic and threadsafe, so calls to `Bun.randomUUIDv7()` from many Workers in the same process at the same timestamp don’t produce colliding counter values.
The final 8 bytes of the UUID are a cryptographically secure random value. It uses the same random number generator used by `crypto.randomUUID()` (which comes from BoringSSL, which in turn comes from the platform-specific system random number generator usually provided by the underlying hardware).
`"buffer"` as the encoding to get a 16-byte buffer instead of a string. This can avoid string conversion overhead.
buffer.ts

`base64` and `base64url` encodings are also supported when you want a slightly shorter string.
base64.ts

`Bun.peek()`

`Bun.peek(prom: Promise)`
Reads a promise’s result without `await` or `.then`, but only if the promise has already fulfilled or rejected.
`peek.status` reads the status of a promise without resolving it.
`Bun.openInEditor()`

Opens a file in your default editor. Bun auto-detects your editor from the `$VISUAL` or `$EDITOR` environment variables.
`debug.editor` setting in your `bunfig.toml`.
bunfig.toml

`editor` param. You can also specify a line and column number.
`Bun.deepEquals()`

Recursively checks if two objects are equivalent. `expect().toEqual()` in `bun:test` uses this internally.
`expect().toStrictEqual()` in the test runner uses this.
`Bun.escapeHTML()`

`Bun.escapeHTML(value: string | object | number | boolean): string`
Escapes the following characters from an input string:
- `"`becomes- `"`
- `&`becomes- `&`
- `'`becomes- `'`
- `<`becomes- `<`
- `>`becomes- `>`

`Bun.stringWidth()`

~6,756x faster 

`string-width` alternative`Bun.stringWidth` is ~6,756x faster than the `string-width` npm package for input larger than about 500 characters. Big thanks to sindresorhus for their work on `string-width`.
`Bun.stringWidth` is implemented in native code with SIMD instructions and accounts for Latin1, UTF-16, and UTF-8 encodings. It passes `string-width`’s tests.
View full benchmark

View full benchmark

1 nanosecond (ns) is 1 billionth of a second. For converting between units:

| Unit | 1 Millisecond | 
|---|---|
| ns | 1,000,000 | 
| µs | 1,000 | 
| ms | 1 | 

terminal

terminal

`Bun.fileURLToPath()`

Converts a `file://` URL to an absolute path.
`Bun.pathToFileURL()`

Converts an absolute path to a `file://` URL.
`Bun.gzipSync()`

Compresses a `Uint8Array` using zlib’s GZIP algorithm.
zlib compression options

zlib compression options

`Bun.gunzipSync()`

Decompresses a `Uint8Array` using zlib’s GUNZIP algorithm.
`Bun.deflateSync()`

Compresses a `Uint8Array` using zlib’s DEFLATE algorithm.
`Bun.gzipSync`.
`Bun.inflateSync()`

Decompresses a `Uint8Array` using zlib’s INFLATE algorithm.
`Bun.zstdCompress()` / `Bun.zstdCompressSync()`

Compresses a `Uint8Array` using the Zstandard algorithm.
`Bun.zstdDecompress()` / `Bun.zstdDecompressSync()`

Decompresses a `Uint8Array` using the Zstandard algorithm.
`Bun.inspect()`

Serializes an object to a `string` exactly as it would be printed by `console.log`.
`Bun.inspect.custom`

The symbol Bun uses to implement `Bun.inspect`. Override it to customize how your objects are printed. It is identical to `util.inspect.custom` in Node.js.
`Bun.inspect.table(tabularData, properties, options)`

Format tabular data into a string. Like `console.table`, except it returns a string rather than printing to the console.
`{ colors: true }` to enable ANSI colors.
`Bun.nanoseconds()`

Returns the number of nanoseconds since the current `bun` process started, as a `number`. Useful for high-precision timing and benchmarking.
`Bun.readableStreamTo*()`

Bun implements a set of convenience functions for asynchronously consuming the body of a `ReadableStream` and converting it to various binary formats.
`Bun.resolveSync()`

Resolves a file path or module specifier using Bun’s internal module resolution algorithm. The first argument is the path to resolve, and the second argument is the “root”. If no match is found, it throws an `Error`.
`process.cwd()` or `"."` as the root.
`import.meta.dir`.
`Bun.stripANSI()`

~6-57x faster 

`strip-ansi` alternative`Bun.stripANSI(text: string): string`
Strip ANSI escape codes from a string. Use it to remove colors and formatting from terminal output.
`Bun.stripANSI` is faster than the `strip-ansi` npm package:
terminal

terminal

`Bun.wrapAnsi()`

Drop-in replacement for 

`wrap-ansi` npm package`Bun.wrapAnsi(input: string, columns: number, options?: WrapAnsiOptions): string`
Wrap text to a specified column width. It preserves ANSI escape codes and hyperlinks and handles Unicode/emoji width correctly. This is a native alternative to the `wrap-ansi` npm package.
### Options

| Option | Default | Description | 
|---|---|---|
| `hard` | `false` | If `true`, break words in the middle if they exceed the column width. | 
| `wordWrap` | `true` | If `true`, wrap at word boundaries. If`false`, only break at explicit newlines. | 
| `trim` | `true` | If `true`, trim leading and trailing whitespace from each line. | 
| `ambiguousIsNarrow` | `true` | If `true`, treat ambiguous-width Unicode characters as 1 column wide. If`false`, treat them as 2 columns wide. | 

`serialize` & `deserialize` in `bun:jsc`

To save a JavaScript value into an ArrayBuffer & back, use `serialize` and `deserialize` from the `"bun:jsc"` module.
`structuredClone` and `postMessage` serialize and deserialize the same way. This exposes the underlying HTML Structured Clone Algorithm to JavaScript as an ArrayBuffer.
`estimateShallowMemoryUsageOf` in `bun:jsc`

The `estimateShallowMemoryUsageOf` function returns a best-effort estimate of the memory usage of an object in bytes, excluding the memory usage of properties or other objects it references. For accurate per-object memory usage, use `Bun.generateHeapSnapshot`.

# Citations

1. Source page: https://bun.sh/docs/runtime/utils
