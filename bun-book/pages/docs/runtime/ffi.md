---
type: Web Page
title: FFI - Bun
description: Use Bun's FFI module to efficiently call native libraries from JavaScript
resource: https://bun.sh/docs/runtime/ffi
timestamp: '2026-08-03T08:59:43.078871+00:00'
---

`bun:ffi` module to efficiently call native libraries from JavaScript. It works with any language that supports the C ABI, including Zig, Rust, C/C++, C#, Nim, and Kotlin.
## dlopen usage (`bun:ffi`)

To print the version number of `sqlite3`:
## Performance

According to
[our benchmark](https://github.com/oven-sh/bun/tree/main/bench/ffi),

`bun:ffi` is roughly 2-6x faster than Node.js FFI through `Node-API`.
`dlopen`, `linkSymbols`, `CFunction`, and `JSCallback` are implemented natively by Bun’s JavaScript engine (JavaScriptCore): argument conversion, arity handling, and result boxing happen in-engine, and hot call sites compile down through the DFG/FTL JIT tiers into direct native calls with no per-argument JavaScript shim. [TinyCC](https://github.com/TinyCC/tinycc), a small and fast C compiler, is embedded only for

[, which compiles C source you provide at runtime.](/docs/runtime/c-compiler)

`cc()`
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
`buffer_length` is `buffer`’s length twin: pass the **same**

`TypedArray`/`DataView` you passed
for the `buffer` parameter, and the callee receives that view’s **byte length**as an unsigned 64-bit integer. The engine reads the pointer and the length off the same object at the moment of the call, so the two always agree — an atomic snapshot you can’t get by passing

`view.byteLength` yourself (a length read in JavaScript beforehand can go stale against a
resizable, growable, or transferred buffer). It’s argument-only and, like the napi types, not
available inside `cc()`.
`napi_env` and `napi_value` are only valid in [source, where a](/docs/runtime/c-compiler)

`cc()``napi_env` parameter is filled in with the module’s environment by the compiled trampoline (the
JavaScript argument passed at that position is a placeholder and is ignored) and `napi_value`
passes the JavaScript value through unchanged. Using either type in a `dlopen`, `linkSymbols`, `JSCallback`,
or `CFunction` descriptor throws a `TypeError`.
## Strings

JavaScript strings and C-like strings are different, and that complicates using strings with native libraries.
## How are JavaScript strings and C strings different?

JavaScript strings:

- UTF16 (2 bytes per letter) or potentially latin1, depending on the JavaScript engine & what characters are used
- `length` stored separately
- Immutable

- UTF8 (1 byte per letter), usually
- The length is not stored. Instead, the string is null-terminated: its length is the index of the first `\0`
- Mutable

`bun:ffi` exports `CString`, which reads a UTF-8 C string at a pointer and returns a plain JavaScript string:
`new CString()` returns a normal string (`typeof myString === "string"`, `myString === "hello"` works) that is a clone of the C string, so it is safe to continue using it after `ptr` has been freed.
`returns`, `FFIType.cstring` coerces the pointer to a JavaScript `string`. When used in `args`, `FFIType.cstring` accepts everything `ptr` does **and**additionally accepts a JavaScript string directly — the engine transcodes it to a null-terminated UTF-8 buffer that lives for the duration of the call, so you don’t need to encode it into a

`Buffer` yourself:
**Lifetime of a**The pointer is whatever the C function returned — memory owned by the native side (a static, a buffer it manages, or heap it allocated); the engine copies nothing on return, and the JavaScript string is cloned out of it. The one aliasing case is a C function that hands back a pointer

`cstring` return.
*derived from a*: that argument was transcoded into the engine’s call-scoped buffer, so treat such a returned pointer as valid only until your next FFI call reuses that buffer (the usual C rule for functions that return their input). Clone it (via the returned string, or

`cstring` argument you passed as a JavaScript
string`new CString`) rather
than holding the raw address.
## Function pointers

Async functions are not supported

`CFunction`, for example with a pointer you got from a Node-API (napi) module you’ve already loaded.
`linkSymbols`:
## Callbacks

Use`JSCallback` to create JavaScript callback functions that you can pass to C/FFI functions, so native code can call back into your JavaScript or TypeScript. This is useful for asynchronous code.
`JSCallback`, call `close()` to free the memory.
### Experimental thread-safe callbacks

`JSCallback` has experimental support for thread-safe callbacks. You need this if you pass a callback function into a different thread from the one that created it. Enable it with the optional `threadsafe` parameter.
Thread-safe callbacks can be invoked from **any thread**— including threads spawned by your native library that Bun is not otherwise aware of. The engine copies the C arguments on the calling thread and marshals the invocation onto the JavaScript thread, where the arguments are converted (64-bit integers and pointers arrive as exact BigInts) and your function runs. Because the invocation is asynchronous from C’s point of view, the value returned to the C caller is unspecified: you may declare a non-

`void` `returns` (the example below uses `"bool"`), but the C side must treat a thread-safe callback as returning `void` and ignore its return value.
**⚡️ Performance tip**: For a slight performance boost, pass

`JSCallback.prototype.ptr` directly instead of the `JSCallback` object:
## Pointers

Bun represents
[pointers](<https://en.wikipedia.org/wiki/Pointer_(computer_programming)>)as a

`number` in JavaScript.
## How does a 64 bit pointer fit in a JavaScript number?

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
## Hardmode

Hardmode

If you don’t want the automatic conversion, or you want a pointer to a specific byte offset within the 

`TypedArray`, get the pointer to the `TypedArray` directly:

# Citations

1. Source page: https://bun.sh/docs/runtime/ffi
