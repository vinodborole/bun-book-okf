---
type: Web Page
title: Metrics - Bun
description: Monitor server activity with built-in metrics
resource: https://bun.sh/docs/runtime/http/metrics
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

Documentation IndexFetch the complete documentation index at: /docs/llms.txtUse this file to discover all available pages before exploring further.

Fetch the complete documentation index at: /docs/llms.txt

Use this file to discover all available pages before exploring further.

Monitor server activity with built-in metrics

server.pendingRequests

server.pendingWebSockets

const server = Bun.serve({ fetch(req, server) { return new Response( `Active requests: ${server.pendingRequests}\n` + `Active WebSockets: ${server.pendingWebSockets}`, ); }, });

server.subscriberCount(topic)

const server = Bun.serve({ fetch(req, server) { const chatUsers = server.subscriberCount("chat"); return new Response(`${chatUsers} users in chat`); }, websocket: { message(ws) { ws.subscribe("chat"); }, }, });

Was this page helpful?

# Citations

1. Source page: https://bun.sh/docs/runtime/http/metrics
