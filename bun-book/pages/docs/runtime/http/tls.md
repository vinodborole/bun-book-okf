---
type: Web Page
title: TLS | Bun Docs
description: Enable TLS in Bun.serve
resource: https://bun.sh/docs/runtime/http/tls
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# TLS

Enable TLS in Bun.serve

Bun's TLS support is built in, powered by [BoringSSL](https://boringssl.googlesource.com/boringssl). To enable TLS, pass both `key` and `cert`.

```
Bun.serve({
  tls: {
    key: Bun.file("./key.pem"), 
    cert: Bun.file("./cert.pem"), 
  },
});
```
The `key` and `cert` fields expect the *contents* of your TLS key and certificate, *not a path to it*. Each can be a string, `BunFile`, `TypedArray`, `Buffer`, or an array of those. Bun uses only the last key/cert pair in an array; to serve several certificates, pass an array of `tls` objects, each with a `serverName` (see [SNI](#server-name-indication-sni) below).

```
Bun.serve({
  tls: {
    key: Bun.file("./key.pem"), // BunFile
    key: fs.readFileSync("./key.pem"), // Buffer
    key: fs.readFileSync("./key.pem", "utf8"), // string
    key: [Bun.file("./key1.pem"), Bun.file("./key2.pem")], // array of above
  },
});
```
### Passphrase

If your private key is encrypted with a passphrase, provide a value for `passphrase` to decrypt it.

```
Bun.serve({
  tls: {
    key: Bun.file("./key.pem"),
    cert: Bun.file("./cert.pem"),
    passphrase: "my-secret-passphrase", 
  },
});
```
### CA Certificates

Pass `ca` to override the trusted CA certificates. By default, the server trusts the list of well-known CAs curated by Mozilla; setting `ca` replaces that list.

```
Bun.serve({
  tls: {
    key: Bun.file("./key.pem"), // path to TLS key
    cert: Bun.file("./cert.pem"), // path to TLS cert
    ca: Bun.file("./ca.pem"), // path to root CA certificate
  },
});
```
### Diffie-Hellman

To override Diffie-Hellman parameters:

```
Bun.serve({
  tls: {
    dhParamsFile: "/path/to/dhparams.pem", // path to Diffie Hellman parameters
  },
});
```
## Server name indication (SNI)

To configure the server name indication (SNI) for the server, set the `serverName` field in the `tls` object.

```
Bun.serve({
  tls: {
    serverName: "my-server.com", // SNI
  },
});
```
To allow multiple server names, pass an array of objects to `tls`, each with a `serverName` field.

```
Bun.serve({
  tls: [
    {
      key: Bun.file("./key1.pem"),
      cert: Bun.file("./cert1.pem"),
      serverName: "my-server1.com", 
    },
    {
      key: Bun.file("./key2.pem"),
      cert: Bun.file("./cert2.pem"),
      serverName: "my-server2.com", 
    },
  ],
});
```

# Citations

1. Source page: https://bun.sh/docs/runtime/http/tls
