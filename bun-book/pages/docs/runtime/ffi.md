---
type: Web Page
title: FFI - Bun
description: Use Bun's FFI module to efficiently call native libraries from JavaScript
resource: https://bun.sh/docs/runtime/ffi
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

`bun:ffi` module to efficiently call native libraries from JavaScript. It works with any language that supports the C ABI, including Zig, Rust, C/C++, C#, Nim, and Kotlin.
## dlopen usage (`bun:ffi`)

To print the version number of `sqlite3`:
## Performance

According to[our benchmark](https://github.com/oven-sh/bun/tree/main/bench/ffi),

`bun:ffi` is roughly 2-6x faster than Node.js FFI through `Node-API`.
Bun generates and just-in-time compiles C bindings that efficiently convert values between JavaScript types and native types. To compile C, Bun embeds [TinyCC](https://github.com/TinyCC/tinycc), a small and fast C compiler.

## Usage

### Zig

add.zig

terminal

`dlopen`:
### Rust

### C++

## FFI types

The following`FFIType` values are supported.
`buffer` arguments must be a `TypedArray` or `DataView`.
## Strings

JavaScript strings and C-like strings are different, and that complicates using strings with native libraries.How are JavaScript strings and C strings different?

How are JavaScript strings and C strings different?

JavaScript strings:

- UTF16 (2 bytes per letter) or potentially latin1, depending on the JavaScript engine & what characters are used
- `length`stored separately
- Immutable

- UTF8 (1 byte per letter), usually
- The length is not stored. Instead, the string is null-terminated: its length is the index of the first `\0`
- Mutable

`bun:ffi` exports `CString` which extends JavaScript’s built-in `String` to support null-terminated strings and add a few extras:
`new CString()` constructor clones the C string, so it is safe to continue using `myString` after `ptr` has been freed.
`returns`, `FFIType.cstring` coerces the pointer to a JavaScript `string`. When used in `args`, `FFIType.cstring` is identical to `ptr`.
## Function pointers

Async functions are not supported

`CFunction`, for example with a pointer you got from a Node-API (napi) module you’ve already loaded.
`linkSymbols`:
## Callbacks

Use`JSCallback` to create JavaScript callback functions that you can pass to C/FFI functions, so native code can call back into your JavaScript or TypeScript. This is useful for asynchronous code.
`JSCallback`, call `close()` to free the memory.
### Experimental thread-safe callbacks

`JSCallback` has experimental support for thread-safe callbacks. You need this if you pass a callback function into a different thread from the one that created it. Enable it with the optional `threadsafe` parameter.
Thread-safe callbacks work best when run from another thread that is running JavaScript code, that is, a [. A future version of Bun will enable them to be called from any thread, such as new threads spawned by your native library that Bun is not aware of.](/docs/runtime/workers)

`Worker`**⚡️ Performance tip**: For a slight performance boost, pass

`JSCallback.prototype.ptr` directly instead of the `JSCallback` object:## Pointers

Bun represents[pointers](https://en.wikipedia.org/wiki/Pointer_(computer_programming))as a

`number` in JavaScript.
How does a 64 bit pointer fit in a JavaScript number?

How does a 64 bit pointer fit in a JavaScript number?

64-bit processors support up to 

[52 bits of addressable space](https://en.wikipedia.org/wiki/64-bit_computing#Limits_of_processors).[JavaScript numbers](https://en.wikipedia.org/wiki/Double-precision_floating-point_format#IEEE_754_double-precision_binary_floating-point_format:_binary64)support 53 bits of usable space, which leaves about 11 bits of extra space.**Why not**`BigInt`?`BigInt` is slower. JavaScript engines allocate `BigInt`s separately, so they can’t fit into a regular JavaScript value. If you pass a `BigInt` to a function, it is converted to a `number`.**Windows Note**: The Windows API type HANDLE does not represent a virtual address, and using`ptr` for it does *not*work as expected. Use`u64` to safely represent HANDLE values.`TypedArray` to a pointer:
`ArrayBuffer`:
`DataView`:
`read`:
`read` function behaves similarly to `DataView`, but it’s usually faster because it doesn’t need to create a `DataView` or `ArrayBuffer`.
### Memory management

`bun:ffi` does not manage memory for you. You must free the memory when you’re done with it.
#### From JavaScript

To track when a`TypedArray` is no longer in use from JavaScript, use a [FinalizationRegistry](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/FinalizationRegistry).

#### From C, Rust, Zig, etc

To track when a`TypedArray` is no longer in use from C or FFI, pass a callback and an optional context pointer to `toArrayBuffer` or `toBuffer`. The callback is called later, once the garbage collector frees the underlying `ArrayBuffer` JavaScript object.
The expected signature is the same as in [JavaScriptCore’s C API](https://developer.apple.com/documentation/javascriptcore/jstypedarraybytesdeallocator?language=objc):

### Memory safety

Don’t use raw pointers outside of FFI. A future version of Bun may add a CLI flag to disable`bun:ffi`.
### Pointer alignment

If an API expects a pointer sized to something other than`char` or `u8`, make sure the `TypedArray` is also that size. A `u64*` is not exactly the same as `[8]u8*` due to alignment.
### Passing a pointer

Where FFI functions expect a pointer, pass a`TypedArray` of equivalent size:
[auto-generated wrapper](https://github.com/oven-sh/bun/blob/6a65631cbdcae75bfa1e64323a6ad613a922cd1a/src/bun.js/ffi.exports.js#L180-L182)converts the

`TypedArray` to a pointer.
Hardmode

Hardmode

If you don’t want the automatic conversion, or you want a pointer to a specific byte offset within the 

`TypedArray`, get the pointer to the `TypedArray` directly:

# Citations

1. Source page: https://bun.sh/docs/runtime/ffi
