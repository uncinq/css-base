---
isIndex: false
title: Select
description: A select with its own chevron, the padding that keeps text off it, and the multiple case that drops it.
weight: 21
icon: menu-button-wide
---

`base/form-select.css` styles `select` and `optgroup`. Largely inspired by [KNACSS](https://knacss.com/).

```css
select {
  appearance: none;
  background-image: var(--select-icon);
  background-position: right var(--select-icon-size) center;
  background-repeat: no-repeat;
  background-size: var(--select-icon-size);
  border: 0;
  box-shadow: inset 0 0 0 var(--select-border-width) var(--select-color-border);
  padding: var(--select-padding);
  padding-inline-end: var(--select-padding-end);
  width: 100%;
}
```

| Token | Role |
| --- | --- |
| `--select-color-background` | Background |
| `--select-color-border` | Border colour, drawn as an inset shadow |
| `--select-color-border-hover` | Hover border colour |
| `--select-border-width` | Border thickness |
| `--select-border-radius` | Corner radius |
| `--select-color-text` | Text colour |
| `--select-icon` | The chevron, as a `url()` or `image-set()` |
| `--select-icon-size` | Chevron size, and the inset it sits at |
| `--select-padding` | Padding |
| `--select-padding-end` | Inline-end padding, wider to clear the chevron |
| `--select-color-optgroup` | `optgroup` label colour |

## The chevron is a background image, not a mask

Unlike the [details marker](../details/#the-summary-and-its-marker) or the [checkbox tick](../form-checkbox/#the-tick-is-a-mask), this glyph is a `background-image`. A select already uses its background for the control surface, and an element has one `mask` but can layer several backgrounds, so the image is the workable option here.

The consequence is that the chevron does **not** follow `currentColor`. Its colour is baked into `--select-icon`, so a dark theme has to supply a different token value rather than relying on the text colour changing. Both values live in [@uncinq/component-tokens](../../../component-tokens/reference/).

`--select-icon-size` does double duty, as the glyph size and as the inset in `background-position: right var(--select-icon-size) center`. One token, and the chevron stays proportionally spaced at any size.

## Two paddings

`padding` sets all four sides, then `padding-inline-end` widens the end side alone so a long option label stops before it reaches the chevron rather than running underneath it. Keep `--select-padding-end` larger than `--select-icon-size` plus its inset.

## `[multiple]` is a different control

```css
&[multiple] {
  background-image: none;
  cursor: default;
  padding-inline-end: var(--size-16, 1rem);
}
```

A multiple select renders as a list box, not as a dropdown. There is nothing to drop down, so the chevron is removed, the pointer cursor goes back to the default, and the extra end padding is no longer needed.

## `appearance: none` removes more than the arrow

It also removes the platform focus ring and, on some platforms, the disabled rendering. Both are put back explicitly: `:focus-visible` uses the shared `--focus-*` ring, and `:disabled` drops to `--opacity-disabled`.

`user-select: none` stops a double click from selecting the option label as text, which is never what the click was for.

## The dropdown list itself is not stylable

`<option>` rendering is owned by the operating system in most browsers. This file styles `optgroup` labels, with `--select-color-optgroup` and a bold weight, and leaves the options alone. A fully styled list means a [`.dropdown` component](../../../css-components/overlays/dropdown/) with its own markup and its own JavaScript, which is a different control with a different accessibility contract.
