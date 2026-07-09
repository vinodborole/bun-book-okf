---
type: Web Page
title: JSONL - Bun
description: Parse newline-delimited JSON (JSONL) with Bun's built-in streaming parser
resource: https://bun.sh/docs/runtime/jsonl
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

[JSONL](https://jsonlines.org/)(newline-delimited JSON), where each line is a separate JSON value. The parser is implemented in C++ using JavaScriptCore’s optimized JSON parser and supports streaming.

`Bun.JSONL.parse()`

Parse a complete JSONL input and return an array of all parsed values.
`Uint8Array`:
`Uint8Array` input, Bun skips a UTF-8 BOM at the start of the buffer.
### Error handling

If the input contains invalid JSON and no values were successfully parsed,`Bun.JSONL.parse()` throws a `SyntaxError`. If at least one value was parsed before the error, the parsed values are returned without throwing.
`Bun.JSONL.parseChunk()`

For streaming, `parseChunk` parses as many complete values as it can from the input and reports how far it got, so you know where to resume when data arrives incrementally (for example, from a network stream).
### Return value

`parseChunk` returns an object with four properties:
| Property | Type | Description | 
|---|---|---|
| `values` | `any[]` | Array of successfully parsed JSON values | 
| `read` | `number` | Number of bytes (for `Uint8Array`) or characters (for strings) consumed | 
| `done` | `boolean` | `true`if the entire input was consumed with no remaining data | 
| `error` | `SyntaxError | null` | Parse error, or `null`if no error occurred | 

### Streaming example

Use`read` to slice off consumed input and carry forward the remainder:
### Byte offsets with `Uint8Array`

When the input is a `Uint8Array`, you can pass optional `start` and `end` byte offsets:
`read` value is always a byte offset into the original buffer. Use it with `TypedArray.subarray()` for zero-copy streaming:
### Error recovery

Unlike`parse()`, `parseChunk()` does not throw on invalid JSON. Instead, it returns the error in the `error` property, along with any values that were successfully parsed before the error:
## Supported value types

Each line can be any valid JSON value, not just objects:## Performance notes

- **ASCII fast path**: Pure ASCII input is parsed directly without copying, using a zero-allocation- `StringView`.
- **UTF-8 support**: Non-ASCII- `Uint8Array`input is decoded to UTF-16 using SIMD-accelerated conversion.
- **BOM handling**: UTF-8 BOM (- `0xEF 0xBB 0xBF`) at the start of a- `Uint8Array`is automatically skipped.
- **Pre-built object shape**: The result object from- `parseChunk`uses a cached structure for fast property access.

# Citations

1. Source page: https://bun.sh/docs/runtime/jsonl
