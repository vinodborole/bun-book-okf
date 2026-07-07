---
type: Web Page
title: Cookies - Bun
description: Use Bun's native APIs for working with HTTP cookies
resource: https://bun.sh/docs/runtime/cookies
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`Bun.Cookie` and `Bun.CookieMap`. They parse, generate, and manipulate cookies in HTTP requests and responses.
## CookieMap class

`Bun.CookieMap` is a Map-like collection of cookies. It implements `Iterable`, so it works with `for...of` loops and the other iteration methods.
cookies.ts

### In HTTP servers

In Bun’s HTTP server, the`cookies` property on the request object (in `routes`) is an instance of `CookieMap`:
server.ts

### Methods

`get(name: string): string | null`

Retrieves a cookie by name. Returns `null` if the cookie doesn’t exist.
get-cookie.ts

`has(name: string): boolean`

Checks if a cookie with the given name exists.
has-cookie.ts

`set(name: string, value: string): void`

`set(options: CookieInit): void`

`set(cookie: Cookie): void`

Adds or updates a cookie in the map. Cookies default to `{ path: "/", sameSite: "lax" }`.
set-cookie.ts

`delete(name: string): void`

`delete(options: CookieStoreDeleteOptions): void`

Removes a cookie from the map. When applied to a Response, this adds a cookie with an empty string value and an expiry date in the past. The browser only deletes the cookie if the domain and path match the ones it was created with.
delete-cookie.ts

`toJSON(): Record<string, string>`

Converts the cookie map to a serializable format.
cookie-to-json.ts

`toSetCookieHeaders(): string[]`

Returns an array of values for Set-Cookie headers that apply all cookie changes.
Use this with HTTP servers other than `Bun.serve()`. In `Bun.serve()`, you don’t need to call it: any changes made to the `req.cookies` map are automatically applied to the response headers.
node-server.js

### Iteration

`CookieMap` provides several methods for iteration:
iterate-cookies.ts

### Properties

`size: number`

Returns the number of cookies in the map.
cookie-size.ts

## Cookie class

`Bun.Cookie` represents an HTTP cookie with its name, value, and attributes.
cookie-class.ts

### Constructors

constructors.ts

### Properties

cookie-properties.ts

### Methods

`isExpired(): boolean`

Checks if the cookie has expired. When both `maxAge` and `expires` are set, `maxAge` takes precedence, as required by RFC 6265.
is-expired.ts

`serialize(): string`

`toString(): string`

Returns a string representation of the cookie suitable for a `Set-Cookie` header.
serialize-cookie.ts

`toJSON(): CookieInit`

Converts the cookie to a plain object suitable for JSON serialization.
cookie-json.ts

### Static methods

`Cookie.parse(cookieString: string): Cookie`

Parses a cookie string into a `Cookie` instance.
parse-cookie.ts

`Cookie.from(name: string, value: string, options?: CookieInit): Cookie`

Factory method to create a cookie.
cookie-from.ts

## Types

types.ts

# Citations

1. Source page: https://bun.sh/docs/runtime/cookies
