---
type: Web Page
title: Server - Bun
description: Use Bun.serve to start a high-performance HTTP server in Bun
resource: https://bun.sh/docs/runtime/http/server
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

## Basic Setup

index.ts

## HTML imports

Import HTML files directly into your server code to build full-stack applications with both server-side and client-side code. HTML imports work in two modes:**Development (**Bun bundles assets on demand at runtime and enables hot module replacement (HMR): when you change your frontend code, the browser updates without a full page reload.

`bun --hot`):**Production (**When you build with

`bun build`):`bun build --target=bun`, the `import index from "./index.html"` statement resolves to a pre-built manifest object containing all bundled client assets. `Bun.serve` serves the assets from this manifest with no bundling at runtime.
[bundler](/docs/bundler), JavaScript transpiler, and CSS parser, so you can build frontends with React, TypeScript, and Tailwind CSS. For a complete guide to building full-stack applications with HTML imports, see

[fullstack dev server](/docs/bundler/fullstack).

## Configuration

### Changing the `port` and `hostname`

To configure which port and hostname the server listens on, set `port` and `hostname` in the options object.
`port` to `0`.
`port` or `url` property.
### Configuring a default port

Several flags and environment variables set the default port, which Bun uses when the`port` option is not set.
- `--port`CLI flag

- `BUN_PORT`environment variable

- `PORT`environment variable

terminal

- `NODE_PORT`environment variable

terminal

## Unix domain sockets

To listen on a[unix domain socket](https://en.wikipedia.org/wiki/Unix_domain_socket), pass the

`unix` option with the path to the socket.
### Abstract namespace sockets

On Linux, Bun also supports abstract namespace sockets: prefix the`unix` path with a null byte.
## HTTP/3 (QUIC)

HTTP/3 support in 

`Bun.serve` is **experimental**and may change in future releases.`Bun.serve` can also listen for HTTP/3 over QUIC. Set `http3: true` together with [; HTTP/3 requires TLS.](/docs/runtime/http/tls)

`tls``http3` is enabled, the server listens on the same port over both TCP (HTTP/1.1) and UDP (HTTP/3). HTTP/1.1 responses include an `Alt-Svc` header advertising the HTTP/3 endpoint so capable clients can upgrade automatically.
To serve HTTP/3 only — no TCP listener at all — set `http1: false`:
`http3` is not supported with unix domain sockets — QUIC requires a UDP port. `http1: false` requires `http3: true`.## idleTimeout

By default,`Bun.serve` closes connections after **10 seconds**of inactivity. A connection is idle when no data is being sent or received, including in-flight requests where your handler is still running but hasn’t written any bytes to the response yet. Browsers and

`fetch()` clients see this as a connection reset.
To configure this, set the `idleTimeout` field (in seconds). The maximum value is `255`, and `0` disables the timeout entirely.
**Streaming & Server-Sent Events**— The idle timer applies while a response is being streamed. If your stream goes quiet for longer than

`idleTimeout`, Bun closes the connection mid-response. For long-lived streams, disable the
timeout for that request with [.](#server-timeout-request-seconds)

`server.timeout(req, 0)`## export default syntax

Instead of passing the server options into`Bun.serve`, you can `export default` them.
server.ts

`<undefined>` is the WebSocket data type. If you add a `websocket` handler that attaches custom data with `server.upgrade(req, { data: ... })`, replace `undefined` with your data type.
You can run this file as-is: when Bun sees a file with a `default` export containing a `fetch` handler, it passes it into `Bun.serve`.
## Hot Route Reloading

Update routes without server restarts using`server.reload()`:
## Server Lifecycle Methods

`server.stop()`

To stop the server from accepting new connections:
`stop()` allows in-flight requests and WebSocket connections to complete. Pass `true` to immediately terminate all connections.
`server.ref()` and `server.unref()`

Control whether the server keeps the Bun process alive:
`server.reload()`

Update the server’s handlers without restarting:
`fetch`, `error`, `routes`, and `websocket` can be updated.
## Per-Request Controls

`server.timeout(Request, seconds)`

Override the idle timeout for an individual request. Pass `0` to disable the timeout entirely for that request.
`server.timeout(req, 0)` to keep a long-lived streaming response (like Server-Sent Events) alive without raising the global `idleTimeout` for every request:
`server.requestIP(Request)`

Get client IP and port information:
`null` for closed requests or Unix domain sockets.
## Server Metrics

`server.pendingRequests` and `server.pendingWebSockets`

Monitor server activity with built-in counters:
`server.subscriberCount(topic)`

Get count of subscribers for a WebSocket topic:
## Benchmarks

The following Bun and Node.js servers respond`Bun!` to each incoming `Request`.
Bun

`Bun.serve` server can handle roughly 2.5x more requests per second than Node.js on Linux.
## Practical example: REST API

Here’s a basic database-backed REST API using Bun’s router with zero dependencies:## Reference

See TypeScript Definitions

# Citations

1. Source page: https://bun.sh/docs/runtime/http/server
