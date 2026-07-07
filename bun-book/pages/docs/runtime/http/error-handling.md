---
type: Web Page
title: Error Handling - Bun
description: Learn how to handle errors in Bun's development server
resource: https://bun.sh/docs/runtime/http/error-handling
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`development: true`.
server.ts

`error` callback

To handle server-side errors, implement an `error` handler. Return a `Response` to serve to the client when an error occurs. In `development` mode, this response replaces Bun’s default error page.

# Citations

1. Source page: https://bun.sh/docs/runtime/http/error-handling
