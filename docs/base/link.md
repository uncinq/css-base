---
isIndex: false
title: Links
description: Colour, underline and focus ring on a, and the automatic marker on target="_blank".
weight: 5
icon: link-45deg
---

`base/link.css` styles `a` directly. There is no `.link` class.

```css
a {
  color: var(--color-link);
  text-underline-offset: var(--text-decoration-offset);
  text-decoration-color: var(--link-color-text-decoration);
  text-decoration-line: var(--link-text-decoration-line);
  text-decoration-thickness: var(--text-decoration-thickness);
}
```

| Token | Role |
| --- | --- |
| `--color-link` | Link colour |
| `--color-link-hover` | Hover colour |
| `--link-text-decoration-line` | `underline` by default, set to `none` to drop it |
| `--link-color-text-decoration` | Underline colour, independent of the text colour |
| `--link-color-text-decoration-hover` | Underline colour on hover |
| `--text-decoration-offset` | Distance from the baseline |
| `--text-decoration-thickness` | Underline weight |
| `--link-transition` | The transition, applied only under `prefers-reduced-motion: no-preference` |

Declaring the underline colour separately from the text colour is what lets a link keep a full-contrast label with a lighter underline, which reads as a link without shouting.

## External links get a marker

An `a[target="_blank"]` grows an icon on `::after`, masked from `--icon-external-link`:

```css
&[target="_blank"]::after {
  background-color: currentColor;
  content: '';
  mask: var(--icon-external-link) center / var(--icon-size) no-repeat;
  /* … */
}
```

Because it is a mask over `currentColor` rather than a background image, the marker follows the link's colour through every state, including hover. Size it with `--icon-size` and space it with `--icon-margin-inline-start`, which falls back to `--spacing-2xs`.

The marker is **decoration, not an accessible name**. A link that opens in a new tab still needs to say so in its text or in a `.visually-hidden` span, since the icon is a `::after` pseudo-element and is not exposed to assistive technology. See [accessibility helpers](../accessibility/).

To opt one link out:

```css
.no-marker[target="_blank"]::after {
  content: none;
}
```

## Focus and hover

```css
&:focus-visible {
  outline: var(--focus-outline-width) var(--focus-outline-style) var(--focus-color-outline);
  outline-offset: var(--focus-outline-offset);
}
```

The focus ring uses the same `--focus-*` tokens as every control in the layer, so a keyboard user sees one ring everywhere. Hover is guarded by `@media (hover: hover) and (pointer: fine)`, so tapping a link on a touch screen does not leave it in its hover colour.
