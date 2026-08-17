---
type: Web Page
title: File System Router | Bun Docs
description: Bun provides a fast API for resolving routes against file-system paths
resource: https://bun.sh/docs/runtime/file-system-router
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# File System Router

Bun provides a fast API for resolving routes against file-system paths

This API is intended primarily for library authors. It supports only Next.js-style file-system routing.

## Next.js-style

The `FileSystemRouter` class resolves routes against a `pages` directory. (The Next.js 13 `app` directory is not supported.) Consider the following `pages` directory:

```
pages
├── index.tsx
├── settings.tsx
├── blog
│   ├── [slug].tsx
│   └── index.tsx
└── [[...catchall]].tsx
```
To resolve routes against this directory:

router.ts

```
const router = new Bun.FileSystemRouter({
  style: "nextjs",
  dir: "./pages",
  origin: "https://mydomain.com",
  assetPrefix: "_next/static/"
});
router.match("/");
// =>
{
  filePath: "/path/to/pages/index.tsx",
  kind: "exact",
  name: "/",
  pathname: "/",
  src: "https://mydomain.com/_next/static/index.tsx"
}
```
The router parses query parameters and returns them in the `query` property.

```
router.match("/settings?foo=bar");
// =>
{
  filePath: "/Users/colinmcd94/Documents/bun/fun/pages/settings.tsx",
  kind: "exact",
  name: "/settings",
  pathname: "/settings?foo=bar",
  src: "https://mydomain.com/_next/static/settings.tsx",
  query: {
    foo: "bar"
  }
}
```
The router parses URL parameters and returns them in the `params` property:

```
router.match("/blog/my-cool-post");
// =>
{
  filePath: "/Users/colinmcd94/Documents/bun/fun/pages/blog/[slug].tsx",
  kind: "dynamic",
  name: "/blog/[slug]",
  pathname: "/blog/my-cool-post",
  src: "https://mydomain.com/_next/static/blog/[slug].tsx",
  params: {
    slug: "my-cool-post"
  }
}
```
The `.match()` method also accepts `Request` and `Response` objects; the router uses their `url` property to resolve the route.

`router.match(new Request("https://example.com/blog/my-cool-post"));`
The router reads the directory contents on initialization. To re-scan the files, use the `.reload()` method.

`router.reload();`
## Reference

```
interface Bun {
  class FileSystemRouter {
    constructor(params: {
      dir: string;
      style: "nextjs";
      origin?: string;
      assetPrefix?: string;
      fileExtensions?: string[];
    });
    reload(): void;
    match(path: string | Request | Response): {
      filePath: string;
      kind: "exact" | "catch-all" | "optional-catch-all" | "dynamic";
      name: string;
      pathname: string;
      src: string;
      params?: Record<string, string>;
      query?: Record<string, string>;
    } | null
  }
}
```

# Citations

1. Source page: https://bun.sh/docs/runtime/file-system-router
