---
type: Web Page
title: CSRF Protection - Bun
description: Generate and verify CSRF tokens with Bun's built-in API
resource: https://bun.sh/docs/runtime/csrf
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

`Bun.CSRF` generates and verifies [CSRF (Cross-Site Request Forgery)](https://owasp.org/www-community/attacks/csrf)tokens. Tokens are signed with HMAC and include an expiration timestamp.

csrf.ts

Always pass a 

`sessionId` (the requester’s session identifier or user ID) to both `generate()` and `verify()`. Without
it, a token is only bound to the secret — any token the server has ever issued validates for every user, so an
attacker can obtain a token in their own session and replay it in a forged cross-site request from a victim’s browser.`Bun.CSRF.generate()`

Generate a CSRF token. The token contains a cryptographic nonce, a timestamp, and an HMAC signature, encoded as a string.
generate.ts

**Parameters:**

- `secret`(string, optional) — The secret key used to sign the token. If not provided, Bun generates a random in-memory default secret (unique per thread).
- `options`(object, optional):

**Returns:**

`string` — the encoded token.
generate-options.ts

`Bun.CSRF.verify()`

Verify a CSRF token. Returns `true` if the token is valid and has not expired, `false` otherwise.
verify.ts

**Parameters:**

- `token`(string, required) — The token to verify.
- `options`(object, optional):

**Returns:**

`boolean`
verify-options.ts

## Using with `Bun.serve()`

A typical pattern is to generate a token when rendering a form, embed it in a hidden field, and verify it when the form is submitted. Pass the requester’s session identifier as `sessionId` to both calls so the token only works for the user it was issued to.
server.ts

## Default secret

If you omit the`secret` parameter in both `generate()` and `verify()`, Bun uses a random secret generated once per thread. This is convenient for single-threaded applications, but tokens won’t verify across servers or workers, or after a restart.
default-secret.ts

## TypeScript

types.ts

# Citations

1. Source page: https://bun.sh/docs/runtime/csrf
