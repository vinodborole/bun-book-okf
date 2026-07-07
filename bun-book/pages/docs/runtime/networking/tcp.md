---
type: Web Page
title: TCP - Bun
description: Use Bun's native TCP API to implement performance-sensitive systems like
  database clients, game servers, or anything that needs to communicate over TCP (instead
  of HTTP)
resource: https://bun.sh/docs/runtime/networking/tcp
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

## Start a server (`Bun.listen()`)

Start a TCP server with `Bun.listen`:
server.ts

An API designed for speed

An API designed for speed

In Bun, you declare one set of handlers per server instead of assigning callbacks to each socket, as with Node.js For performance-sensitive servers, assigning listeners to each socket can cause significant garbage collector pressure and increase memory usage. By contrast, Bun only allocates one handler function for each event and shares it among all sockets. This is a small optimization, but it adds up.

`EventEmitters` or the web-standard `WebSocket` API.server.ts

`open` handler.
server.ts

`tls` object containing `key` and `cert` fields.
server.ts

`key` and `cert` fields expect the *contents*of your TLS key and certificate. This can be a string,

`BunFile`, `TypedArray`, or `Buffer`.
server.ts

`Bun.listen` returns a server that conforms to the `TCPSocket` interface.
server.ts

## Create a connection (`Bun.connect()`)

Use `Bun.connect` to connect to a TCP server. Specify the server with `hostname` and `port`. TCP clients can define the same set of handlers as `Bun.listen`, plus a few client-specific handlers.
server.ts

`tls: true`.
## Hot reloading

Both TCP servers and sockets can be hot reloaded with new handlers.## Buffering

TCP sockets in Bun do not buffer data, so performance-sensitive code should buffer writes itself. For example, this:`ArrayBufferSink` with the `{stream: true}` option:
server.ts

**Corking**Support for corking is planned, but in the meantime backpressure must be managed manually with the

`drain` handler.

# Citations

1. Source page: https://bun.sh/docs/runtime/networking/tcp
