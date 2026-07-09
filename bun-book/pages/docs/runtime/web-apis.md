---
type: Web Page
title: Web APIs - Bun
description: Web-standard APIs supported by Bun for server-side JavaScript
resource: https://bun.sh/docs/runtime/web-apis
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

[DOM API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API#html_dom_api_interfaces)and

[History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API), aren’t relevant in a server-first runtime like Bun. Many others are broadly useful outside the browser; when possible, Bun implements these Web-standard APIs instead of introducing new ones. The following Web APIs are partially or completely supported.

| Category | APIs | 
|---|---|
| HTTP | ,`fetch`,`Response`,`Request`,`Headers`,`AbortController``AbortSignal` | 
| URLs | ,`URL``URLSearchParams` | 
| Web Workers | ,`Worker`,`self.postMessage`,`structuredClone`,`MessagePort`,`MessageChannel``BroadcastChannel` | 
| Streams | ,`ReadableStream`,`WritableStream`,`TransformStream`,`ByteLengthQueuingStrategy`and associated classes`CountQueuingStrategy` | 
| Blob | `Blob` | 
| WebSockets | `WebSocket` | 
| Encoding and decoding | ,`Uint8Array`,`Uint8Array.prototype.toBase64()`,`Uint8Array.fromBase64()`,`TextEncoder`,`TextDecoder`,`atob``btoa` | 
| JSON | `JSON` | 
| Timeouts | ,`setTimeout``clearTimeout` | 
| Intervals | ,`setInterval``clearInterval` | 
| Crypto | ,`crypto`,`SubtleCrypto``CryptoKey` | 
| Debugging | ,`console``performance` | 
| Microtasks | `queueMicrotask` | 
| Errors | `reportError` | 
| User interaction | ,`alert`,`confirm`(intended for interactive CLIs)`prompt` | 
| Realms | `ShadowRealm` | 
| Events | ,`EventTarget`,`Event`,`ErrorEvent`,`CloseEvent``MessageEvent` |

# Citations

1. Source page: https://bun.sh/docs/runtime/web-apis
