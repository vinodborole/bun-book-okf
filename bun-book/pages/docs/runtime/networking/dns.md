---
type: Web Page
title: DNS - Bun
description: Use Bun's DNS module to resolve DNS records
resource: https://bun.sh/docs/runtime/networking/dns
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`dns` module and the `node:dns` module.
## DNS caching in Bun

Bun caches DNS lookups, which makes repeated connections to the same hosts faster. The cache holds up to 256 entries for a maximum of 30 seconds each. If a connection to a host fails, Bun removes that host’s entry from the cache. Simultaneous connections to the same host share one DNS lookup. This cache is automatically used by:- `bun install`
- `fetch()`
- `node:http`(client)
- `Bun.connect`
- `node:net`
- `node:tls`

### When should I prefetch a DNS entry?

Web browsers expose[to resolve a hostname before it’s needed. In Bun,](https://developer.mozilla.org/en-US/docs/Web/Performance/dns-prefetch)

`<link rel="dns-prefetch">``dns.prefetch` does the same thing: use it when you know you’ll connect to a host soon and want to avoid the initial DNS lookup.
`dns.prefetch`

`dns.prefetch` resolves a hostname before you need it.
`dns.getCacheStats()`

`dns.getCacheStats()` returns the current cache stats as an object with the following properties:
### Configuring DNS cache TTL

Bun caches DNS entries for 30 seconds by default. To change the TTL, set the`$BUN_CONFIG_DNS_TIME_TO_LIVE_SECONDS` environment variable. For example, to set it to 5 seconds:
#### Why is 30 seconds the default?

The system API underneath (`getaddrinfo`) does not expose the TTL of a DNS entry, so Bun has to pick a number. We chose 30 seconds because it’s long enough to see the benefits of caching and short enough to be unlikely to cause issues if a DNS entry changes. [Amazon Web Services recommends 5 seconds](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/jvm-ttl-dns.html)for the Java Virtual Machine, though the JVM’s default is to cache indefinitely.

# Citations

1. Source page: https://bun.sh/docs/runtime/networking/dns
