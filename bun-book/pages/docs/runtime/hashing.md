---
type: Web Page
title: Hashing - Bun
description: Utility functions for hashing and verifying passwords with various cryptographically
  secure algorithms
resource: https://bun.sh/docs/runtime/hashing
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

Bun implements the 

`createHash` and `createHmac` functions from [in addition to the Bun-native APIs documented below.](https://nodejs.org/api/crypto.html)`node:crypto``Bun.password`

`Bun.password` is a collection of utility functions for hashing and verifying passwords with various cryptographically secure algorithms.
`Bun.password.hash` is a params object that selects and configures the hashing algorithm.
`bcrypt`, the returned hash is encoded in [Modular Crypt Format](https://passlib.readthedocs.io/en/stable/modular_crypt_format.html)for compatibility with most existing

`bcrypt` implementations; with `argon2` the result is encoded in the newer [PHC format](https://github.com/P-H-C/phc-string-format/blob/master/phc-sf-spec.md). The

`verify` function detects the algorithm from the input hash, whether PHC- or MCF-encoded, and uses the matching verification method.
### Salt

`Bun.password.hash` generates a salt automatically and includes it in the hash.
### bcrypt - Modular Crypt Format

In the following[Modular Crypt Format](https://passlib.readthedocs.io/en/stable/modular_crypt_format.html)hash (used by

`bcrypt`):
Input:
- `bcrypt`:- `$2b`
- `rounds`:- `$10`- rounds (log2 of the actual number of rounds)
- `salt`:- `Lyj9kHYZtiyfxh2G60TEfe`
- `hash`:- `qs7xkkGiEFFDi3iJGc50ZG/XJ1sxIFi`

`Bun.password.hash` with the `bcrypt` algorithm hashes any password longer than 72 bytes with SHA-512 before passing it to bcrypt.
### argon2 - PHC format

In the following[PHC format](https://github.com/P-H-C/phc-string-format/blob/master/phc-sf-spec.md)hash (used by

`argon2`):
Input:
- `algorithm`:- `$argon2id`
- `version`:- `$v=19`
- `memory cost`:- `65536`
- `iterations`:- `t=2`
- `parallelism`:- `p=1`
- `salt`:- `$xXnlSvPh4ym5KYmxKAuuHVlDvy2QGHBNuI6bJJrRDOs`
- `hash`:- `$2YY6M48XmHn+s5NoBaL+ficzXajq2Yj8wut3r0vnrwI`

`Bun.hash`

`Bun.hash` is a collection of utilities for *non-cryptographic*hashing. Non-cryptographic hashing algorithms are optimized for speed of computation over collision-resistance or security. The standard

`Bun.hash` function uses [Wyhash](https://github.com/wangyi-fudan/wyhash)to generate a 64-bit hash from an input of arbitrary size.

`TypedArray`, `DataView`, `ArrayBuffer`, or `SharedArrayBuffer`.
`Number.MAX_SAFE_INTEGER` as BigInt to avoid loss of precision.
`Bun.hash`. The API is the same for each; 32-bit hashes return a number and 64-bit hashes return a bigint.
`Bun.CryptoHasher`

`Bun.CryptoHasher` incrementally computes a hash of string or binary data with a cryptographic hash algorithm. The following algorithms are supported:
- `"blake2b256"`
- `"blake2b512"`
- `"blake2s256"`
- `"md4"`
- `"md5"`
- `"ripemd160"`
- `"sha1"`
- `"sha224"`
- `"sha256"`
- `"sha384"`
- `"sha512"`
- `"sha512-224"`
- `"sha512-256"`
- `"sha3-224"`
- `"sha3-256"`
- `"sha3-384"`
- `"sha3-512"`
- `"shake128"`
- `"shake256"`

`.update()`, which accepts `string`, `TypedArray`, and `ArrayBuffer`.
`'utf-8'`). The following encodings are supported:
`.digest()`. By default, this method returns a `Uint8Array` containing the hash.
`.digest()`:
`.digest()` can write the hash into an existing `TypedArray` instead of allocating a new one.
### HMAC in `Bun.CryptoHasher`

`Bun.CryptoHasher` can compute HMAC digests. Pass the key to the constructor.
- `"blake2b256"`
- `"blake2b512"`
- `"md4"`
- `"md5"`
- `"ripemd160"`
- `"sha1"`
- `"sha224"`
- `"sha256"`
- `"sha384"`
- `"sha512"`
- `"sha512-224"`
- `"sha512-256"`
- `"sha3-224"`
- `"sha3-256"`
- `"sha3-384"`
- `"sha3-512"`

`Bun.CryptoHasher`, the HMAC `Bun.CryptoHasher` instance is not reset after `.digest()` is called, and using the same instance again throws an error.
Other methods like `.copy()` and `.update()` are supported (as long as it’s before `.digest()`), but methods like `.digest()` that finalize the hasher are not.

# Citations

1. Source page: https://bun.sh/docs/runtime/hashing
