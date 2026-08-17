---
type: Web Page
title: Error Handling | Bun Docs
description: Learn how to handle errors in Bun's development server
resource: https://bun.sh/docs/runtime/http/error-handling
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# Error Handling

Learn how to handle errors in Bun's development server

To activate development mode, set `development: true`.

server.ts

```
Bun.serve({
  development: true, 
  fetch(req) {
    throw new Error("woops!");
  },
});
```
In development mode, Bun surfaces errors in-browser with a built-in error page.

### `error` callback

To handle server-side errors, implement an `error` handler. Return a `Response` to serve to the client when an error occurs. In `development` mode, this response replaces Bun's default error page.

```
Bun.serve({
  fetch(req) {
    throw new Error("woops!");
  },
  error(error) {
    return new Response(`<pre>${error}\n${error.stack}</pre>`, {
      headers: {
        "Content-Type": "text/html",
      },
    });
  },
});
```

# Citations

1. Source page: https://bun.sh/docs/runtime/http/error-handling
