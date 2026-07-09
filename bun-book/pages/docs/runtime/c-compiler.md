---
type: Web Page
title: C Compiler - Bun
description: Compile and run C from JavaScript with low overhead
resource: https://bun.sh/docs/runtime/c-compiler
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`bun:ffi` has experimental support for compiling and running C from JavaScript with low overhead.
## Usage (cc in `bun:ffi`)

See the [introduction blog post](https://bun.com/blog/compile-and-run-c-in-js)for background. JavaScript:

hello.ts

hello.c

`hello.ts` prints:
terminal

`cc` uses [TinyCC](https://bellard.org/tcc/)to compile the C code, then links it with the JavaScript runtime, converting types in-place.

### Primitive types

`cc` supports the same `FFIType` values as [.](/docs/runtime/ffi)

`dlopen`| `FFIType` | C Type | Aliases | 
|---|---|---|
| cstring | `char*` | |
| function | `(void*)(*)()` | `fn`,`callback` | 
| ptr | `void*` | `pointer`,`void*`,`char*` | 
| i8 | `int8_t` | `int8_t` | 
| i16 | `int16_t` | `int16_t` | 
| i32 | `int32_t` | `int32_t`,`int` | 
| i64 | `int64_t` | `int64_t` | 
| i64_fast | `int64_t` | |
| u8 | `uint8_t` | `uint8_t` | 
| u16 | `uint16_t` | `uint16_t` | 
| u32 | `uint32_t` | `uint32_t` | 
| u64 | `uint64_t` | `uint64_t` | 
| u64_fast | `uint64_t` | |
| f32 | `float` | `float` | 
| f64 | `double` | `double` | 
| bool | `bool` | |
| char | `char` | |
| napi_env | `napi_env` | |
| napi_value | `napi_value` | 

### Strings, objects, and non-primitive types

For strings, objects, and other non-primitive types that don’t map 1:1 to C types,`cc` supports N-API.
Use `napi_value` to pass or receive JavaScript values from a C function without any type conversions.
You can also pass a `napi_env` to receive the N-API environment used to call the JavaScript function.
#### Returning a C string to JavaScript

For example, to return a string from C to JavaScript:hello.ts

hello.c

hello.c

`cc` Reference

`library: string[]`

Use the `library` array to specify the libraries to link with the C code.
`symbols`

Use the `symbols` object to specify the functions and variables to expose to JavaScript.
`source`

`source` is the path to the C code to compile and link with the JavaScript runtime.
`flags: string | string[]`

`flags` is an optional array of strings passed to the TinyCC compiler.
`-I` for include directories and `-D` for preprocessor definitions.
`define: Record<string, string>`

`define` is an optional object of preprocessor definitions passed to the TinyCC compiler.

# Citations

1. Source page: https://bun.sh/docs/runtime/c-compiler
