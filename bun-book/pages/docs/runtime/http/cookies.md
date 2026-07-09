---
type: Web Page
title: Cookies - Bun
description: Work with cookies in HTTP requests and responses using Bun's built-in
  Cookie API.
resource: https://bun.sh/docs/runtime/http/cookies
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`BunRequest` object exposes a `cookies` property, a `CookieMap` for reading and modifying cookies. When using `routes`, `Bun.serve()` automatically tracks calls to `request.cookies.set` and applies them to the response.
## Reading cookies

Read cookies from incoming requests using the`cookies` property on the `BunRequest` object:
## Setting cookies

To set cookies, use the`set` method on the `CookieMap` from the `BunRequest` object.
## Deleting cookies

To delete a cookie, use the`delete` method on the `request.cookies` (`CookieMap`) object:
`Set-Cookie` header on the response with the `Expires` attribute set to a date in the past and an empty `value`.

# Citations

1. Source page: https://bun.sh/docs/runtime/http/cookies
