---
isIndex: false
title: Body
description: The six tokens on body that every other element inherits from.
weight: 2
icon: window
---

`base/body.css` sets the six properties that everything else inherits. It is the shortest file in the layer and the one with the widest reach.

```css
body {
  background-color: var(--color-background);
  color: var(--color-text);
  font-family: var(--font-family-text);
  font-size: var(--font-size-text, 1rem);
  font-weight: var(--font-weight-text, 400);
  line-height: var(--line-height-text, 1.5);
}
```

| Token | Sets |
| --- | --- |
| `--color-background` | Page background |
| `--color-text` | Default text colour |
| `--font-family-text` | Body typeface |
| `--font-size-text` | Base font size, falls back to `1rem` |
| `--font-weight-text` | Base weight, falls back to `400` |
| `--line-height-text` | Base line-height, falls back to `1.5` |

The last three carry a literal fallback on purpose. The upstream reset this package is based on sets `line-height: 1.5` on `body`; here that rule is [commented out](../../reset/#deviations-from-the-original-reset) and the value comes from a token instead, with the fallback preserving the original behaviour when the token is missing.

`--color-background` and `--color-text` are the two that the dark theme overlay swaps. Nothing in this file mentions a colour scheme: the theme redefines the semantic tokens and the body follows. See [dark mode in design-tokens](../../../design-tokens/dark-mode/).

## Overriding

```css
@layer base {
  :root {
    --font-size-text: 1.125rem;
  }
}
```

Changing `--font-size-text` moves the whole document, because every `em`-based and inherited size in the layer is measured against it. To change only one element, set its own token instead.
