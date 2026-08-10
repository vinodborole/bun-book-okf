---
type: Web Page
title: HTMLRewriter - Bun
description: Use Bun's HTMLRewriter to transform HTML documents with CSS selectors
resource: https://bun.sh/docs/runtime/html-rewriter
timestamp: '2026-08-10T07:07:25.236908+00:00'
---

`Response`, `string`, and `ArrayBuffer` inputs. Bun’s implementation is based on Cloudflare’s [lol-html](https://github.com/cloudflare/lol-html).

## Usage

A common use case is rewriting URLs in HTML content:`<img>` in a link, producing a diff like this:
[a very famous video](https://www.youtube.com/watch?v=dQw4w9WgXcQ).

### Input types

HTMLRewriter can transform HTML from several input types:`Response` objects.
### Element Handlers

The`on(selector, handlers)` method registers handlers for HTML elements that match a CSS selector. The handlers run for each matching element during parsing:
`transform(response)` returns immediately; the rewrite continues in the
background and you read the result off the returned `Response`. Because the
rewrite outlives `transform()`, an error thrown by an async handler (or a
Promise it returns that rejects) rejects the response body instead of throwing
from `transform()`:
`transform()` on a `string` or `ArrayBuffer` has to return its result
synchronously, so it cannot wait for a handler that needs the event loop to turn
(a timer, I/O, a `fetch`). Such a handler makes `transform()` throw a
`TypeError`, and the rewrite fails without running any further handlers:
`process.nextTick` and already-resolved
Promises) still works with `transform(string)`. Pass a `Response` whenever a
handler might await real work.
### CSS Selector Support

The`on()` method supports a wide range of CSS selectors:
### Element Operations

All element modification methods return the element instance, so calls can be chained:
### Text Operations

Text chunks represent portions of text content and report their position in the text node:
### Comment Operations

Comments support similar methods to text nodes:
### Document Handlers

The`onDocument(handlers)` method registers handlers for events at the document level rather than within specific elements:
### Response Handling

When transforming a Response:
- The status code, headers, and other response properties are preserved
- The body is transformed while maintaining streaming capabilities
- Content-encoding (like gzip) is handled automatically
- The original response body is marked as used after transformation
- Headers are cloned to the new response

## Error Handling

Which channel an error takes is decided by the overload you called, never by timing.`transform()` itself throws for:
- Invalid selector syntax in the `on()` method
- Invalid input types (for example, passing a Symbol)
- Body already used errors, and input bodies that have already failed or aborted
- Anything a content handler raises on a `string` /`ArrayBuffer` input, since
those have to produce their result before`transform()` returns — including a
handler that needs the event loop (see[Element Handlers](#element-handlers) )

`Response` input, `transform()` returns before the rewrite finishes, so
everything the rewrite discovers surfaces on the output body instead:
- An error thrown by a content handler, or a rejected Promise one returned
- Malformed or truncated input
- Stream errors reading the input body
- Memory allocation failures

`unhandledRejection` path. Earlier versions of Bun could surface
it from `transform()` itself.
## See also

You can also read the
[Cloudflare documentation](https://developers.cloudflare.com/workers/runtime-apis/html-rewriter/), which this API is intended to be compatible with.

# Citations

1. Source page: https://bun.sh/docs/runtime/html-rewriter
