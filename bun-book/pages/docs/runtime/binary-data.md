---
type: Web Page
title: Binary Data - Bun
description: Working with binary data in JavaScript
resource: https://bun.sh/docs/runtime/binary-data
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

| Class | Description | 
|---|---|
| `TypedArray` | A family of classes that provide an `Array`-like interface for interacting with binary data. Includes`Uint8Array`,`Uint16Array`,`Int8Array`, and more. | 
| `Buffer` | A subclass of `Uint8Array`that implements a wide range of convenience methods. Unlike the others in this table, it’s a Node.js API (which Bun implements). It isn’t available in the browser. | 
| `DataView` | A class that provides a `get/set`API for writing some number of bytes to an`ArrayBuffer`at a particular byte offset. Often used for reading or writing binary protocols. | 
| `Blob` | A readonly blob of binary data usually representing a file. Has a MIME `type`, a`size`, and methods for converting to`ArrayBuffer`,`ReadableStream`, and string. | 
| `File` | A subclass of `Blob`that represents a file. Has a`name`and`lastModified`timestamp. Node.js v20 has experimental support. | 
| `BunFile` | Bun only. A subclass of`Blob`that represents a lazily-loaded file on disk. Created with`Bun.file(path)`. | 

`ArrayBuffer` and views

JavaScript had no language-native way to store and manipulate binary data until ECMAScript v5 (2009) introduced a range of mechanisms for it. The most fundamental building block is `ArrayBuffer`, a data structure that represents a sequence of bytes in memory.
`ArrayBuffer` directly; all you can do is check its size and create “slices” from it.
*wraps*an

`ArrayBuffer` instance and lets you read and manipulate the underlying data. There are two types of views: *typed arrays*and

`DataView`.
`DataView`

The `DataView` class is a lower-level interface for reading and manipulating the data in an `ArrayBuffer`.
The following creates a `DataView` and sets the first byte to 3.
`Uint16` at byte offset `1`. This requires two bytes. The value `513` is `2 * 256 + 1`; in bytes, that’s `00000010 00000001`.
`ArrayBuffer` now have values. Even though the second and third bytes were written with `setUint16()`, you can still read each component byte with `getUint8()`.
`ArrayBuffer` has throws an error. The following writes a `Float64` (which requires 8 bytes) at byte offset `0`, but the buffer is only four bytes long.
`DataView`:
`TypedArray`

Typed arrays are a family of classes that provide an `Array`-like interface for interacting with data in an `ArrayBuffer`. Whereas a `DataView` lets you write numbers of varying size at a particular offset, a `TypedArray` interprets the underlying bytes as an array of numbers, each of a fixed size.
It’s common to refer to this family of classes collectively by their shared superclass 

`TypedArray`. This class is
*internal*to JavaScript; you can’t directly create instances of it, and`TypedArray` is not defined in the global
scope. Think of it as an `interface` or an abstract class.`ArrayBuffer`:
| Class | Description | 
|---|---|
| `Uint8Array` | Every one (1) byte is interpreted as an unsigned 8-bit integer. Range 0 to 255. | 
| `Uint16Array` | Every two (2) bytes are interpreted as an unsigned 16-bit integer. Range 0 to 65535. | 
| `Uint32Array` | Every four (4) bytes are interpreted as an unsigned 32-bit integer. Range 0 to 4294967295. | 
| `Int8Array` | Every one (1) byte is interpreted as a signed 8-bit integer. Range -128 to 127. | 
| `Int16Array` | Every two (2) bytes are interpreted as a signed 16-bit integer. Range -32768 to 32767. | 
| `Int32Array` | Every four (4) bytes are interpreted as a signed 32-bit integer. Range -2147483648 to 2147483647. | 
| `Float16Array` | Every two (2) bytes are interpreted as a 16-bit floating point number. Range -6.104e5 to 6.55e4. | 
| `Float32Array` | Every four (4) bytes are interpreted as a 32-bit floating point number. Range -3.4e38 to 3.4e38. | 
| `Float64Array` | Every eight (8) bytes are interpreted as a 64-bit floating point number. Range -1.7e308 to 1.7e308. | 
| `BigInt64Array` | Every eight (8) bytes are interpreted as a signed `BigInt`. Range -9223372036854775808 to 9223372036854775807 (though`BigInt`is capable of representing larger numbers). | 
| `BigUint64Array` | Every eight (8) bytes are interpreted as an unsigned `BigInt`. Range 0 to 18446744073709551615 (though`BigInt`is capable of representing larger numbers). | 
| `Uint8ClampedArray` | Same as `Uint8Array`, but automatically “clamps” to the range 0-255 when assigning a value to an element. | 

`ArrayBuffer` are interpreted by different typed array classes.
| Byte 0 | Byte 1 | Byte 2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7 | |
|---|---|---|---|---|---|---|---|---|
| `ArrayBuffer` | `00000000` | `00000001` | `00000010` | `00000011` | `00000100` | `00000101` | `00000110` | `00000111` | 
| `Uint8Array` | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 
| `Uint16Array` | 256 ( `1 * 256 + 0`) | 770 ( `3 * 256 + 2`) | 1284 ( `5 * 256 + 4`) | 1798 ( `7 * 256 + 6`) | ||||
| `Uint32Array` | 50462976 | 117835012 | ||||||
| `BigUint64Array` | 506097522914230528n | 

`ArrayBuffer`:
`Uint32Array` from this same `ArrayBuffer` throws an error.
`Uint32` value requires four bytes (32 bits). Because the `ArrayBuffer` is 10 bytes long, there’s no way to cleanly divide its contents into 4-byte chunks.
To fix this, create a typed array over a particular “slice” of the `ArrayBuffer`. The following `Uint32Array` only “views” the *first*8 bytes of the underlying

`ArrayBuffer`: a `byteOffset` of `0` and a `length` of `2`, the number of `Uint32` values the array holds.
`ArrayBuffer` instance first; pass a length to the typed array constructor instead:
`push` and `pop` are not available, because they would require resizing the underlying `ArrayBuffer`.
`Uint8Array`

`Uint8Array` is the most common typed array in JavaScript. It represents a classic “byte array”: a sequence of 8-bit unsigned integers between 0 and 255.
In Bun, it has methods for converting between byte arrays and their base64 or hex string representations.
`TextEncoder#encode`, and the input type of `TextDecoder#decode`, two utility classes that translate between strings and various binary encodings, most notably `"utf-8"`.
`Buffer`

Bun implements `Buffer`, a Node.js API for working with binary data that pre-dates the introduction of typed arrays in the JavaScript spec. It has since been re-implemented as a subclass of `Uint8Array`. It provides a wide range of methods, including several Array-like and `DataView`-like methods.
`Blob`

`Blob` is a Web API commonly used for representing files. It originated in browsers (unlike `ArrayBuffer`, which is part of JavaScript itself), but Node.js and Bun support it too.
You rarely create `Blob` instances directly; they usually come from an external source (like an `<input type="file">` element in the browser) or a library. That said, you can create a `Blob` from one or more string or binary “blob parts”.
`string`, `ArrayBuffer`, `TypedArray`, `DataView`, or other `Blob` instances. The parts are concatenated in the order they’re given.
`Blob` asynchronously in various formats.
`BunFile`

`BunFile` is a subclass of `Blob` that represents a lazily-loaded file on disk. Like `File`, it adds a `name` and `lastModified` property. Unlike `File`, it does not require the file to be loaded into memory.
`File`

`File` is a subclass of `Blob` that adds a `name` and `lastModified` property. It’s commonly used in the browser to represent files uploaded with an `<input type="file">` element. Node.js and Bun implement `File`.
## Streams

Streams let you work with binary data without loading it all into memory at once. They’re commonly used for reading and writing files, sending and receiving network requests, and processing large amounts of data. Bun implements the Web APIs`ReadableStream` and `WritableStream`.
To create a readable stream:

`for await`.
## Conversion

Use this section as a reference for converting one binary format to another.### From `ArrayBuffer`

Since `ArrayBuffer` stores the data that underlies other binary structures like `TypedArray`, the following snippets are not *converting*from

`ArrayBuffer` to another format. Instead, they *create*a new instance using the underlying data.

#### To `TypedArray`

#### To `DataView`

#### To `Buffer`

#### To `string`

As UTF-8:
#### To `number[]`

#### To `Blob`

#### To `ReadableStream`

The following snippet creates a `ReadableStream` and enqueues the entire `ArrayBuffer` as a single chunk.
With chunking

With chunking

To stream the 

`ArrayBuffer` in chunks, use a `Uint8Array` view and enqueue each chunk.### From `TypedArray`

#### To `ArrayBuffer`

The `buffer` property is the underlying `ArrayBuffer`. A `TypedArray` can be a view of a *slice*of that buffer, so the sizes may differ.

#### To `DataView`

To create a `DataView` over the same byte range as the `TypedArray`:
#### To `Buffer`

#### To `string`

As UTF-8:
#### To `number[]`

#### To `Blob`

#### To `ReadableStream`

With chunking

With chunking

To stream the 

`ArrayBuffer` in chunks, split the `TypedArray` into chunks and enqueue each one individually.### From `DataView`

#### To `ArrayBuffer`

#### To `TypedArray`

Only works if the `byteLength` of the `DataView` is a multiple of the `BYTES_PER_ELEMENT` of the `TypedArray` subclass.
#### To `Buffer`

#### To `string`

As UTF-8:
#### To `number[]`

#### To `Blob`

#### To `ReadableStream`

With chunking

With chunking

To stream the 

`ArrayBuffer` in chunks, split the `DataView` into chunks and enqueue each one individually.### From `Buffer`

#### To `ArrayBuffer`

#### To `TypedArray`

#### To `DataView`

#### To `string`

As UTF-8:
#### To `number[]`

#### To `Blob`

#### To `ReadableStream`

With chunking

With chunking

To stream the 

`ArrayBuffer` in chunks, split the `Buffer` into chunks and enqueue each one individually.### From `Blob`

#### To `ArrayBuffer`

#### To `TypedArray`

#### To `DataView`

#### To `Buffer`

#### To `string`

As UTF-8:
#### To `number[]`

#### To `ReadableStream`

### From `ReadableStream`

`Response` is a common intermediate for converting a `ReadableStream` to other formats.
`ReadableStream` to various binary formats.
#### To `ArrayBuffer`

#### To `Uint8Array`

#### To `TypedArray`

#### To `DataView`

#### To `Buffer`

#### To `string`

As UTF-8:
#### To `number[]`

`ReadableStream` to an array of its chunks. Each chunk may be a string, typed array, or `ArrayBuffer`.
#### To `Blob`

#### To `ReadableStream`

To split a `ReadableStream` into two streams that can be consumed independently:

# Citations

1. Source page: https://bun.sh/docs/runtime/binary-data
