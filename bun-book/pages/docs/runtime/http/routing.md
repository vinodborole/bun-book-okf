---
type: Web Page
title: Routing - Bun
description: Define routes in Bun.serve using static paths, parameters, and wildcards
resource: https://bun.sh/docs/runtime/http/routing
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`Bun.serve()` with the `routes` property (static paths, parameters, and wildcards), or handle unmatched requests with the [method.](#fetch)

`fetch``Bun.serve()`’s router builds on top of uWebSocket’s [tree-based approach](https://github.com/oven-sh/bun/blob/0d1a00fa0f7830f8ecd99c027fce8096c9d459b6/packages/bun-uws/src/HttpRouter.h#L57-L64)to add

[SIMD-accelerated route parameter decoding](https://github.com/oven-sh/bun/blob/main/src/jsc/bindings/decodeURIComponentSIMD.cpp#L21-L271)and

[JavaScriptCore structure caching](https://github.com/oven-sh/bun/blob/main/src/jsc/bindings/ServerRouteList.cpp#L100-L101)to push the performance limits of what modern hardware allows.

## Basic Setup

server.ts

`Bun.serve()` receive a `BunRequest` (which extends [) and return a](https://developer.mozilla.org/en-US/docs/Web/API/Request)

`Request`[or](https://developer.mozilla.org/en-US/docs/Web/API/Response)

`Response``Promise<Response>`. This makes it easier to use the same code for both sending & receiving HTTP requests.
## Asynchronous Routes

### Async/await

Use async/await in route handlers to return a`Promise<Response>`.
### Promise

You can also return a`Promise<Response>` from a route handler.
## Route precedence

Routes are matched in order of specificity:- Exact routes (`/users/all`)
- Parameter routes (`/users/:id`)
- Wildcard routes (`/users/*`)
- Global catch-all (`/*`)

## Type-safe route parameters

TypeScript parses route parameters when passed as a string literal, so your editor shows autocomplete when accessing`request.params`.
index.ts

`\uFFFD`).
### Static responses

Routes can also be`Response` objects (without the handler function). `Bun.serve()` optimizes them for zero-allocation dispatch, which suits health checks, redirects, and fixed content:
`Response` object.
Static route responses are cached for the lifetime of the server object. To reload static routes, call `server.reload(options)`.
### File Responses vs Static Responses

Serving a file from a route behaves differently depending on whether you buffer the file content or serve it directly:**Static routes**(

`new Response(await file.bytes())`) buffer content in memory at startup:
- **Zero filesystem I/O**during requests - content served entirely from memory
- **ETag support**- Automatically generates and validates ETags for caching
- **If-None-Match**- Returns- `304 Not Modified`when client ETag matches
- **No 404 handling**- Missing files cause startup errors, not runtime 404s
- **Memory usage**- Full file content stored in RAM
- **Best for**: Small static assets, API responses, frequently accessed files

**File routes**(

`new Response(Bun.file(path))`) read from filesystem per request:
- **Filesystem reads**on each request - checks file existence and reads content
- **Built-in 404 handling**- Returns- `404 Not Found`if file doesn’t exist or becomes inaccessible
- **Last-Modified support**- Uses file modification time for- `If-Modified-Since`headers
- **If-Modified-Since**- Returns- `304 Not Modified`when file hasn’t changed since client’s cached version
- **Range request support**- Automatically handles partial content requests with- `Content-Range`headers
- **Streaming transfers**- Uses buffered reader with backpressure handling for efficient memory usage
- **Memory efficient**- Only buffers small chunks during transfer, not entire file
- **Best for**: Large files, dynamic content, user uploads, files that change frequently

## Streaming files

To stream a file, return a`Response` object with a `BunFile` object as the body.
⚡️ 

**Speed**— Bun automatically uses the[system call when possible, enabling zero-copy file transfers in the kernel—the fastest way to send files.](https://man7.org/linux/man-pages/man2/sendfile.2.html)`sendfile(2)`[method on the](https://developer.mozilla.org/en-US/docs/Web/API/Blob/slice)

`slice(start, end)``Bun.file` object. Bun sets the `Content-Range` and `Content-Length` headers on the `Response` object automatically.
`fetch` request handler

The `fetch` handler runs for incoming requests that no route matched. It receives a [object and returns a](https://developer.mozilla.org/en-US/docs/Web/API/Request)

`Request`[or](https://developer.mozilla.org/en-US/docs/Web/API/Response)

`Response`[.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

`Promise<Response>``fetch` handler supports async/await:
`fetch` handler also receives the `Server` object as its second argument.

# Citations

1. Source page: https://bun.sh/docs/runtime/http/routing
