---
type: Web Page
title: Binary Data - Bun
description: Working with binary data in JavaScript
resource: https://bun.sh/docs/runtime/binary-data
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

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
The following table shows how the same bytes in an 

`ArrayBuffer` are interpreted by different typed array classes.
To create a typed array from a pre-defined 

`ArrayBuffer`:
`Uint32Array` from this same `ArrayBuffer` throws an error.
`Uint32` value requires four bytes (32 bits). Because the `ArrayBuffer` is 10 bytes long, there’s no way to cleanly divide its contents into 4-byte chunks.
To fix this, create a typed array over a particular “slice” of the `ArrayBuffer`. The following `Uint32Array` only “views” the *first*8 bytes of the underlying

`ArrayBuffer`: a `byteOffset` of `0` and a `length` of `2`, the number of `Uint32` values the array holds.
`ArrayBuffer` instance first; pass a length to the typed array constructor instead:
`push` and `pop` are not available, because they would require resizing the underlying `ArrayBuffer`.
[MDN documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)for more on typed array properties and methods.

`Uint8Array`

`Uint8Array` is the most common typed array in JavaScript. It represents a classic “byte array”: a sequence of 8-bit unsigned integers between 0 and 255.
In Bun, it has methods for converting between byte arrays and their base64 or hex string representations.
[, and the input type of](https://developer.mozilla.org/en-US/docs/Web/API/TextEncoder)

`TextEncoder#encode`[, two utility classes that translate between strings and various binary encodings, most notably](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder)

`TextDecoder#decode``"utf-8"`.
`Buffer`

Bun implements `Buffer`, a Node.js API for working with binary data that pre-dates the introduction of typed arrays in the JavaScript spec. It has since been re-implemented as a subclass of `Uint8Array`. It provides a wide range of methods, including several Array-like and `DataView`-like methods.
[Node.js documentation](https://nodejs.org/api/buffer.html).

`Blob`

`Blob` is a Web API commonly used for representing files. It originated in browsers (unlike `ArrayBuffer`, which is part of JavaScript itself), but Node.js and Bun support it too.
You rarely create `Blob` instances directly; they usually come from an external source (like an `<input type="file">` element in the browser) or a library. That said, you can create a `Blob` from one or more string or binary “blob parts”.
`string`, `ArrayBuffer`, `TypedArray`, `DataView`, or other `Blob` instances. The parts are concatenated in the order they’re given.
`Blob` asynchronously in various formats.
`BunFile`

`BunFile` is a subclass of `Blob` that represents a lazily-loaded file on disk. Like `File`, it adds a `name` and `lastModified` property. Unlike `File`, it does not require the file to be loaded into memory.
`File`

[is a subclass of](https://developer.mozilla.org/en-US/docs/Web/API/File)

`File``Blob` that adds a `name` and `lastModified` property. It’s commonly used in the browser to represent files uploaded with an `<input type="file">` element. Node.js and Bun implement `File`.
[MDN documentation](https://developer.mozilla.org/en-US/docs/Web/API/Blob).

## Streams

Streams let you work with binary data without loading it all into memory at once. They’re commonly used for reading and writing files, sending and receiving network requests, and processing large amounts of data. Bun implements the Web APIs[and](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)

`ReadableStream`[.](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)

`WritableStream`
To create a readable stream:

`for await`.
[Streams](/docs/runtime/streams).

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

[is a common intermediate for converting a](https://developer.mozilla.org/en-US/docs/Web/API/Response)

`Response``ReadableStream` to other formats.
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
