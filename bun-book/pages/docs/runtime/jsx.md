---
type: Web Page
title: JSX - Bun
description: Built-in JSX and TSX support in Bun with configurable transpilation options
resource: https://bun.sh/docs/runtime/jsx
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

`.jsx` and `.tsx` files. Bun’s internal transpiler converts JSX syntax into vanilla JavaScript before execution.
react.tsx

## Configuration

Bun reads your`tsconfig.json` or `jsconfig.json` to determine how to perform the JSX transform internally. If you’d rather not use either, you can set the same options in [. Bun respects the following compiler options.](/docs/runtime/bunfig)

`bunfig.toml``jsx`

How JSX constructs are transformed into vanilla JavaScript internally. The following table lists the possible values of `jsx``jsx`, along with how each transpiles this JSX component:
`jsxFactory`

`jsxFactory`Only applicable when 

`jsx` is `react`.`"React.createElement"`. Set this for libraries like [Preact](https://preactjs.com/)that use a different function name (

`"h"`).
`jsxFragmentFactory`

`jsxFragmentFactory`Only applicable when 

`jsx` is `react`.[JSX fragments](https://react.dev/reference/react/Fragment)such as

`<>Hello</>`. Default value is `"React.Fragment"`.
`jsxImportSource`

`jsxImportSource`Only applicable when 

`jsx` is `react-jsx` or `react-jsxdev`.`createElement`, `jsx`, or `jsxDEV`) is imported from. Default value is `"react"`. You’ll typically need this when using a component library like Preact.
### JSX pragma

You can set any of these values per file with a*pragma*, a comment that sets a compiler option in a particular file.

## Logging

Bun implements special logging for JSX to make debugging easier. Given the following file:index.tsx

## Prop punning

The Bun runtime also supports “prop punning” for JSX: a shorthand for assigning a variable to a prop with the same name.react.tsx

# Citations

1. Source page: https://bun.sh/docs/runtime/jsx
