---
type: Web Page
title: TLS - Bun
description: Enable TLS in Bun.serve
resource: https://bun.sh/docs/runtime/http/tls
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`key` and `cert`.
`key` and `cert` fields expect the *contents*of your TLS key and certificate,

*not a path to it*. Each can be a string,

`BunFile`, `TypedArray`, or `Buffer`.
### Passphrase

If your private key is encrypted with a passphrase, provide a value for`passphrase` to decrypt it.
### CA Certificates

Pass`ca` to override the trusted CA certificates. By default, the server trusts the list of well-known CAs curated by Mozilla; setting `ca` replaces that list.
### Diffie-Hellman

To override Diffie-Hellman parameters:## Server name indication (SNI)

To configure the server name indication (SNI) for the server, set the`serverName` field in the `tls` object.
`tls`, each with a `serverName` field.

# Citations

1. Source page: https://bun.sh/docs/runtime/http/tls
