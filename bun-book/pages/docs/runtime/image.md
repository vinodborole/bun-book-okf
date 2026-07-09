---
type: Web Page
title: Image - Bun
description: Decode, transform, and encode images with a fast native pipeline
resource: https://bun.sh/docs/runtime/image
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`Bun.Image` is a chainable image pipeline for decoding, resizing, rotating, and re-encoding JPEG, PNG, WebP, HEIC, and AVIF — built on libjpeg-turbo, spng, libwebp, and SIMD geometry kernels, with zero npm dependencies and no native addon build step.
[Sharp](https://sharp.pixelplumbing.com/): construct from an input, chain transforms, pick an output format, then

`await` a terminal method. Nothing runs until the terminal is awaited, and the work executes off the JavaScript thread.
## Input

The constructor accepts a path, bytes, or a`Blob` — including `Bun.file()` and `Bun.s3.file()`. `Blob#image()` is shorthand for `new Bun.Image(blob)`:
`Content-Type` are ignored.
**Path strings are filesystem paths.**Don’t pass user-controlled strings directly to the constructor — that’s an arbitrary-file-read primitive. Read untrusted input into a

`Buffer` (with `fetch` or `Bun.file` and your own validation) and pass the bytes.
When passing a `TypedArray`/`ArrayBuffer`, don’t mutate it while a terminal is pending — decode runs off-thread and borrows the bytes. `SharedArrayBuffer` and resizable buffers are refused; use `buf.slice()` to pass a fixed view.
A second `options` argument guards against decompression bombs and controls EXIF handling:
## Metadata

Read`width`, `height`, and `format` without decoding pixel data:
## Resize

| `fit` | Behavior | 
|---|---|
| `"fill"`(default) | Stretch to exactly `width × height` | 
| `"inside"` | Preserve aspect ratio; result fits withinthe box | 

`filter` selects the resampling kernel. The default `"lanczos3"` is the right choice for photographs.
| Filter | Use when | 
|---|---|
| `"lanczos3"`(default) | General-purpose, sharpest for photos | 
| `"lanczos2"` | Slightly softer, fewer ringing artifacts | 
| `"mitchell"` | Smooth gradients; the classic bicubic compromise | 
| `"cubic"` | Catmull-Rom — sharper than Mitchell, can ring | 
| `"mks2013"`/`"mks2021"` | ”Magic Kernel Sharp”; used by Facebook/Instagram | 
| `"bilinear"`/`"linear"` | Fast, soft | 
| `"box"` | Area-average; good for large integer downscales | 
| `"nearest"` | Pixel art / hard edges | 

## Rotate · flip

## Modulate

## Output formats

Calling a format method sets the encode target; without one, the source format is reused.`palette: true` quantizes to a ≤256-color palette and emits an indexed (color-type 3) PNG, optionally with Floyd–Steinberg `dither`. This is typically 3–5× smaller than truecolor for screenshots and UI assets.
## Terminals

A pipeline does no work until one of these is awaited:`.write()` accepts the same destinations as `Bun.write` — a path string, `Bun.file()`, `Bun.s3.file()`, or an fd. If you didn’t chain a format method and the destination is a path string, the extension picks one (`.jpg`/`.png`/`.webp`/`.heic`/`.avif`).
## Placeholders

For a low-quality placeholder to inline in HTML before the real image loads,`.placeholder()` returns a [ThumbHash](https://evanw.github.io/thumbhash/)-rendered ≤32px blur as a

`data:` URL — ~400–700 bytes, no client-side decoder needed:
*itself*, encode a progressive JPEG:

`img.width` and `img.height` reflect the *output*dimensions (they’re

`-1` before).
`Bun.serve` integration

A `Bun.Image` pipeline is a valid `Response` body and sets `Content-Type` automatically. To keep the encode off the JS thread in a server handler, await a terminal first:
`new Response(img)`) also works, but runs the encode synchronously during body init.
## Clipboard

`fromClipboard()` reads PNG, TIFF, HEIC, JPEG, WebP, GIF, or BMP from the system pasteboard on macOS and Windows; the regular decode pipeline takes it from there. Returns `null` if there’s no image, and always `null` on Linux — call `wl-paste`/`xclip` yourself and pass the bytes to the constructor.
For a passive “image in clipboard, press ⌘V” hint, poll `clipboardChangeCount()` (a single integer read) and call `hasClipboardImage()` only when it moves; macOS has no clipboard-change notification, so this is the documented pattern.
## Platform backends

| Linux | macOS | Windows | |
|---|---|---|---|
| JPEG / PNG / WebP | libjpeg-turbo · spng · libwebp | same | same | 
| BMP / GIF (decode) | built-in | ImageIO | WIC | 
| TIFF (decode) | ❌ | ImageIO | WIC | 
| Resize / rotate / flip | Highway SIMD | Accelerate vImage | Highway SIMD | 
| HEIC / AVIF | ❌ `ERR_IMAGE_FORMAT_UNSUPPORTED` | ImageIO ² | WIC ¹ | 
| Clipboard | ❌ returns `null` | NSPasteboard | Win32 | 

**HEIF Image Extensions**/

**AV1 Video Extension**from the Microsoft Store. ² AVIF

*encode*needs an OS AV1 encoder — Apple Silicon M3+ only. Intel Mac and M1/M2 reject with

`ERR_IMAGE_FORMAT_UNSUPPORTED`; AVIF *decode*works everywhere ImageIO does (macOS 13+). When a system-backend format isn’t available on the current machine, the terminal rejects with

`error.code === "ERR_IMAGE_FORMAT_UNSUPPORTED"` — branch on that to fall back to a portable format:
**OS’s**patch level — keep macOS / Windows updated. JPEG, PNG, and WebP go through the same statically-linked codecs on every platform, so encoded output is byte-identical across Linux, macOS, and Windows. To force the portable Highway path for geometry too — e.g. for golden-image tests — set the process-global backend:

# Citations

1. Source page: https://bun.sh/docs/runtime/image
