---
isIndex: false
title: Figures
description: A flex column around the image and its caption, with a caption that turns into a row at 48rem.
weight: 12
icon: image
---

`base/figure.css` lays out `figure` as a flex column and gives `figcaption` a responsive direction.

```html
<figure>
  <picture>
    <img src="…" alt="…">
  </picture>
  <figcaption>
    <p>The caption.</p>
    <span class="credit">© Photographer</span>
  </figcaption>
</figure>
```

| Selector | Role |
| --- | --- |
| `figure` | Flex column, `--figure-gap` between image and caption, `--figure-margin-block` around |
| `figure picture` | Forced to `display: block`, overriding the [`inline-block` default](../picture/) |
| `figure img` | `width: 100%`, `height: auto` |
| `figure figcaption` | Flex column, `--figure-figcaption-font-size`, `--figure-figcaption-gap` |
| `figure p` | `margin-block: 0`, since the figure owns the spacing |
| `picture + figcaption` | `align-items: baseline`, `justify-content: space-between` |

## The caption is a column, then a row

```css
@media (min-width: 48rem) {
  figcaption {
    flex-direction: row;
  }
  .credit {
    margin-inline-start: auto;
  }
}
```

Below 48rem the caption and the credit stack. Above it they sit on one line, with `margin-inline-start: auto` pushing the credit to the far end. The baseline alignment is what keeps the two aligned on their text rather than on their boxes, which matters when the credit is smaller than the caption.

`.credit` is the only class this file expects. It is optional: a caption without one simply fills the row.

## This file uses a literal media query

`@media (min-width: 48rem)` is written out rather than using `@media (--sm)`. It is the only responsive block in the package that does not depend on [postcss-custom-media](../../mediaqueries/#required-build-step), so a figure keeps its responsive caption even in a build with no PostCSS step. `48rem` is the same width `--sm` resolves to.

## `picture` is overridden here

[`base/picture.css`](../picture/) sets `display: inline-block` globally. Inside a figure that would leave the descender gap under the image, so this file sets `display: block` back. The two files are imported in the same layer and `figure picture` is the more specific selector, so the order in `css/base.css` does not matter.
