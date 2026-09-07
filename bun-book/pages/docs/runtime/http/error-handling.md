---
type: Web Page
title: Error Handling | Bun Docs
description: Learn how to handle errors in Bun's development server
resource: https://bun.sh/docs/runtime/http/error-handling
timestamp: '2026-09-07T10:57:18.683997+00:00'
---

# Error Handling

Learn how to handle errors in Bun's development server

`Bun.serve()` runs in development mode by default. It is turned off when `NODE_ENV=production` is set, when Bun is run with `--production`, or when you pass `development: false`.

```
Bun.serve({
  development: false, 
  fetch(req) {
    throw new Error("woops!");
  },
});
```
In development mode, when a request handler throws and no `error` handler returns a response, Bun responds with a built-in error page that includes the error message, stack trace, source code around each frame, and file paths. This is meant for debugging locally.

The development error page sends source code and file paths to whoever made the request. Set `NODE_ENV=production` (or
`development: false`) when deploying so uncaught errors return a plain `500` instead.

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
