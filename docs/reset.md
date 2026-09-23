---
isIndex: false
title: Reset
description: The baseline normalization applied in @layer reset, based on the Josh W Comeau custom CSS reset.
weight: 2
---

`css/reset.css` is a single file scoped to `@layer reset`. It is based on the [Josh W Comeau custom CSS reset](https://www.joshwcomeau.com/css/custom-css-reset/), with a few additions by Un Cinq.

Being the first layer in the recommended order, everything here is the weakest thing in the stylesheet. Any base, layout or component rule overrides it without needing extra specificity.

## What it does

| Rule | Effect |
| --- | --- |
| `box-sizing: border-box` | Applied to every element and pseudo-element |
| `margin: 0` | Removed from everything except `dialog`, whose centering depends on its own margin |
| `interpolate-size: allow-keywords` | Enables animating to and from `auto`, `min-content` and friends. Applied only under `prefers-reduced-motion: no-preference` |
| Text rendering on `body` | `-webkit-font-smoothing: antialiased`, `-moz-osx-font-smoothing: grayscale`, `text-rendering: optimizelegibility`, `font-variant-ligatures: common-ligatures` |
| Media defaults | `display: block` and `max-width: 100%` on `img`, `picture`, `video`, `canvas`, `svg` |
| Form control fonts | `font: inherit` on `input`, `button`, `textarea`, `select` |
| Overflow | `overflow-wrap: break-word` on `p` and `h1` to `h6` |
| Line wrapping | `text-wrap: pretty` on `p`, `text-wrap: balance` on `h1` to `h6` |

## Deviations from the original reset

Two rules from the upstream reset are present but commented out, on purpose.

**`line-height: 1.5` on `body`** is left to the tokens. `css/base/body.css` sets `line-height: var(--line-height-text, 1.5)`, so the value is a design decision rather than a reset concern, and the fallback preserves the original behaviour when the token is missing.

**The root stacking context** (`isolation: isolate` on `#root` or `#__next`) targets framework-specific mount points that do not exist in a framework-agnostic package. Add it in your own project if your framework needs it.

The additions by Un Cinq, absent from the upstream reset, are `-moz-osx-font-smoothing`, `text-rendering`, `font-variant-ligatures`, and the `:not(dialog)` exception on the margin reset.

## Using it alone

```css
@import '@uncinq/css-base/css/reset.css';
```

The reset references no custom property, so unlike the rest of the package it works without the token peer dependencies.
