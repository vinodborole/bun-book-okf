---
type: Web Page
title: Web APIs - Bun
description: Web-standard APIs supported by Bun for server-side JavaScript
resource: https://bun.sh/docs/runtime/web-apis
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

| Category | APIs | 
|---|---|
| HTTP | `fetch`,`Response`,`Request`,`Headers`,`AbortController`,`AbortSignal` | 
| URLs | `URL`,`URLSearchParams` | 
| Web Workers | `Worker`,`self.postMessage`,`structuredClone`,`MessagePort`,`MessageChannel`,`BroadcastChannel` | 
| Streams | `ReadableStream`,`WritableStream`,`TransformStream`,`ByteLengthQueuingStrategy`,`CountQueuingStrategy`and associated classes | 
| Blob | `Blob` | 
| WebSockets | `WebSocket` | 
| Encoding and decoding | `Uint8Array`,`Uint8Array.prototype.toBase64()`,`Uint8Array.fromBase64()`,`TextEncoder`,`TextDecoder`,`atob`,`btoa` | 
| JSON | `JSON` | 
| Timeouts | `setTimeout`,`clearTimeout` | 
| Intervals | `setInterval`,`clearInterval` | 
| Crypto | `crypto`,`SubtleCrypto`,`CryptoKey` | 
| Debugging | `console`,`performance` | 
| Microtasks | `queueMicrotask` | 
| Errors | `reportError` | 
| User interaction | `alert`,`confirm`,`prompt`(intended for interactive CLIs) | 
| Realms | `ShadowRealm` | 
| Events | `EventTarget`,`Event`,`ErrorEvent`,`CloseEvent`,`MessageEvent` | 

Standards & Compatibility

# Web APIs

Web-standard APIs supported by Bun for server-side JavaScript

Some Web APIs, like the DOM API and History API, aren’t relevant in a server-first runtime like Bun. Many others are broadly useful outside the browser; when possible, Bun implements these Web-standard APIs instead of introducing new ones.
The following Web APIs are partially or completely supported.

# Citations

1. Source page: https://bun.sh/docs/runtime/web-apis
