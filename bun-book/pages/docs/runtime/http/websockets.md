---
type: Web Page
title: WebSockets - Bun
description: Server-side WebSockets in Bun
resource: https://bun.sh/docs/runtime/http/websockets
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`Bun.serve()` supports server-side WebSockets, with on-the-fly compression, TLS support, and a Bun-native publish-subscribe API.
**⚡️ 7x more throughput**Bun’s WebSockets are fast. For a

[simple chatroom](https://github.com/oven-sh/bun/tree/main/bench/websocket-server/README.md)on Linux x64, Bun can handle 7x more requests per second than Node.js +

[.](https://github.com/websockets/ws)

`"ws"`| Messages sent per second | Runtime | Clients | 
|---|---|---|
| ~700,000 | ( `Bun.serve`) Bun v0.2.1 (x64) | 16 | 
| ~100,000 | ( `ws`) Node v18.10.0 (x64) | 16 | 

[uWebSockets](https://github.com/uNetworking/uWebSockets).

## Start a WebSocket server

The following server, built with`Bun.serve`, [upgrades](https://developer.mozilla.org/en-US/docs/Web/HTTP/Protocol_upgrade_mechanism)every incoming request to a WebSocket connection in the

`fetch` handler. The socket handlers are declared in the `websocket` parameter.
server.ts

server.ts

An API designed for speed

An API designed for speed

In Bun, handlers are declared once per server, instead of per socket.You pass a single 

`WebSocketHandler` object to `Bun.serve()` with methods for `open`, `message`, `close`, `drain`, and `error`. This is different from the client-side `WebSocket` class, which extends `EventTarget` (`onmessage`, `onopen`, `onclose`).Clients tend to have few socket connections open, so an event-based API makes sense there.But servers tend to have **many**socket connections open, which means:- Time spent adding/removing event listeners for each connection adds up
- Extra memory spent on storing references to callback functions for each connection
- Usually, people create new functions for each connection, which also means more memory

`ServerWebSocket` instance handling the event. The `ServerWebSocket` class is a fast, Bun-native implementation of [with some additional features.](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

`WebSocket`server.ts

### Sending messages

Each`ServerWebSocket` instance has a `.send()` method for sending messages to the client. It supports a range of input types.
server.ts

### Headers

Once the upgrade succeeds, Bun sends a`101 Switching Protocols` response per the [spec](https://developer.mozilla.org/en-US/docs/Web/HTTP/Protocol_upgrade_mechanism). To attach additional

`headers` to this `Response`, pass them to `server.upgrade()`.
server.ts

### Contextual data

Attach contextual`data` to a new WebSocket in the `.upgrade()` call. It is available on the `ws.data` property inside the WebSocket handlers.
To strongly type `ws.data`, add a `data` property to the `websocket` handler object. This types `ws.data` across all lifecycle hooks.
server.ts

Previously, you could specify the type of 

`ws.data` with a type parameter on `Bun.serve`, like `Bun.serve<MyData>({...})`. This pattern was removed due to [a limitation in TypeScript](https://github.com/microsoft/TypeScript/issues/26242)in favor of the`data` property.`WebSocket`.
browser.js

**Identifying users**Cookies set on the page are sent with the WebSocket upgrade request and available on

`req.headers` in the `fetch` handler. Parse them to identify the connecting user and set `data` accordingly.### Pub/Sub

Bun’s`ServerWebSocket` includes a native publish-subscribe API for topic-based broadcasting. Individual sockets can `.subscribe()` to a topic (specified with a string identifier) and `.publish()` messages to all other subscribers to that topic (excluding itself). This topic-based broadcast API is similar to [MQTT](https://en.wikipedia.org/wiki/MQTT)and

[Redis Pub/Sub](https://redis.io/topics/pubsub).

server.ts

`.publish(data)` sends the message to all subscribers of a topic *except*the socket that called

`.publish()`. To send a message to all subscribers of a topic, use the `.publish()` method on the `Server` instance.
### Compression

Enable per-message[compression](https://websockets.readthedocs.io/en/stable/topics/compression.html)with the

`perMessageDeflate` parameter.
server.ts

`boolean` as the second argument to `.send()`.
[Reference](#reference).

### Backpressure

The`.send(message)` method of `ServerWebSocket` returns a `number` indicating the result of the operation.
- `-1`— The message was enqueued but there is backpressure
- `0`— The message was dropped due to a connection issue
- `1+`— The number of bytes sent

### Timeouts and limits

By default, Bun closes a WebSocket connection that has been idle for 120 seconds. Configure this with the`idleTimeout` parameter.
`maxPayloadLength` parameter.
## Connect to a `Websocket` server

Bun implements the `WebSocket` class. To create a WebSocket client that connects to a `ws://` or `wss://` server, create an instance of `WebSocket`, as you would in the browser.
`WebSocket` API.
In Bun, you can also set custom headers directly in the constructor. This is a Bun-specific extension of the `WebSocket` standard. *It does not work in browsers.*

## Reference

See Typescript Definitions

# Citations

1. Source page: https://bun.sh/docs/runtime/http/websockets
