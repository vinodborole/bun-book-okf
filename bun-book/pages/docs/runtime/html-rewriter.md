---
type: Web Page
title: HTMLRewriter - Bun
description: Use Bun's HTMLRewriter to transform HTML documents with CSS selectors
resource: https://bun.sh/docs/runtime/html-rewriter
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`Response`, `string`, and `ArrayBuffer` inputs. Bun’s implementation is based on Cloudflare’s [lol-html](https://github.com/cloudflare/lol-html).

## Usage

A common use case is rewriting URLs in HTML content:`<img>` in a link, producing a diff like this:
[a very famous video](https://www.youtube.com/watch?v=dQw4w9WgXcQ).

### Input types

HTMLRewriter can transform HTML from several input types:`Response` objects.
### Element Handlers

The`on(selector, handlers)` method registers handlers for HTML elements that match a CSS selector. The handlers run for each matching element during parsing:
### CSS Selector Support

The`on()` method supports a wide range of CSS selectors:
### Element Operations

All element modification methods return the element instance, so calls can be chained:### Text Operations

Text chunks represent portions of text content and report their position in the text node:### Comment Operations

Comments support similar methods to text nodes:### Document Handlers

The`onDocument(handlers)` method registers handlers for events at the document level rather than within specific elements:
### Response Handling

When transforming a Response:- The status code, headers, and other response properties are preserved
- The body is transformed while maintaining streaming capabilities
- Content-encoding (like gzip) is handled automatically
- The original response body is marked as used after transformation
- Headers are cloned to the new response

## Error Handling

HTMLRewriter operations can throw errors in several cases:- Invalid selector syntax in `on()`method
- Invalid HTML content in transformation methods
- Stream errors when processing Response bodies
- Memory allocation failures
- Invalid input types (for example, passing a Symbol)
- Body already used errors

## See also

You can also read the[Cloudflare documentation](https://developers.cloudflare.com/workers/runtime-apis/html-rewriter/), which this API is intended to be compatible with.

# Citations

1. Source page: https://bun.sh/docs/runtime/html-rewriter
