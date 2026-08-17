---
type: Web Page
title: Metrics | Bun Docs
description: Monitor server activity with built-in metrics
resource: https://bun.sh/docs/runtime/http/metrics
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# Metrics

Monitor server activity with built-in metrics

### `server.pendingRequests` and `server.pendingWebSockets`

Monitor server activity with built-in counters:

```
const server = Bun.serve({
  fetch(req, server) {
    return new Response(
      `Active requests: ${server.pendingRequests}\n` + `Active WebSockets: ${server.pendingWebSockets}`,
    );
  },
});
```
### `server.subscriberCount(topic)`

Get the number of subscribers for a WebSocket topic:

```
const server = Bun.serve({
  fetch(req, server) {
    const chatUsers = server.subscriberCount("chat");
    return new Response(`${chatUsers} users in chat`);
  },
  websocket: {
    message(ws) {
      ws.subscribe("chat");
    },
  },
});
```

# Citations

1. Source page: https://bun.sh/docs/runtime/http/metrics
