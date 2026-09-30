---
isIndex: false
title: Superscript
description: Raising sup without growing the line box, and why that matters for form labels.
weight: 11
icon: superscript
---

`base/sup.css` replaces the user agent's `vertical-align: super` with a relative offset.

```css
sup {
  font-size: 0.75em;
  inset-block-start: -0.5em;
  line-height: 0;
  margin-inline: 0.1em;
  position: relative;
  vertical-align: baseline;
}
```

## Why `line-height` is zeroed

The user agent default, `vertical-align: super`, raises the glyph **inside** the line box, so the line box grows. A block containing a `<sup>` then becomes taller than one without it.

Zeroing `line-height` takes the superscript out of the line box calculation, and relative positioning raises it without affecting layout. A label carrying a required marker then measures exactly like a label without one, and two fields side by side stay aligned.

```html
<label for="email">Email <sup>*</sup></label>
```

That is the case this file is tuned for. Without it, a two-column [form grid](../../../css-components/forms/form/) shows a one-pixel misalignment between the field that is required and the one that is not, which is visible and has no obvious cause.

## The values are relative

Every value is in `em`, so the superscript scales with whatever text surrounds it. A `<sup>` in an `h1` and one in a footnote both land in the same place proportionally, with no per-context rule.

`vertical-align: baseline` is the reset that makes `inset-block-start` the only thing doing the raising. Leaving `super` in place would apply both offsets.

## Footnotes

The marker is styling only. A footnote reference also needs a link and an accessible name:

```html
<sup><a href="#fn1" id="ref1" aria-describedby="footnotes">1</a></sup>
```
