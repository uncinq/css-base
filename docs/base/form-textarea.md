---
isIndex: false
title: Textarea
description: A textarea that grows with its content through field-sizing, with a 5lh floor and vertical resizing kept.
weight: 23
icon: textarea-resize
---

`base/form-textarea.css` styles `textarea` with the same box as [the text inputs](../form/), plus three declarations specific to a multi-line control. Largely inspired by [KNACSS](https://knacss.com/).

```css
textarea {
  field-sizing: content;
  min-height: var(--textarea-min-height, 5lh);
  resize: vertical;
  /* … the shared input box … */
}
```

| Token | Role |
| --- | --- |
| `--textarea-color-background` | Background |
| `--textarea-color-border` | Border colour, drawn as an inset shadow |
| `--textarea-color-border-hover` | Hover border colour |
| `--textarea-border-width` | Border thickness |
| `--textarea-border-radius` | Corner radius |
| `--textarea-color-text` | Text colour |
| `--textarea-color-placeholder` | Placeholder colour |
| `--textarea-padding` | Padding, falls back to `0.75rem 1rem` |
| `--textarea-min-height` | Minimum height, falls back to `5lh` |

## `field-sizing: content`

The textarea grows as the user types, instead of scrolling inside a fixed box. It is a progressive enhancement: in a browser without support the declaration is ignored and the control behaves exactly as it always has, at `min-height` with a scrollbar.

That is why `min-height` is not optional. With `field-sizing: content` and no floor, an empty textarea collapses to a single line and is indistinguishable from a text input.

## Why the floor is in `lh`

`5lh` is five line heights, so the box is five lines tall **whatever the font size or line-height in scope**. The equivalent in `rem` would have to be recalculated every time either changed, and would be wrong inside any component that sets its own type scale.

## `resize: vertical`

Horizontal resizing is disabled, vertical resizing is kept. A textarea widened past its container breaks the layout around it, while a taller one only takes more vertical space, which the page already handles.

Users who want to resize can still do so, which is the point of leaving `vertical` rather than `none`. With `field-sizing: content` the two coexist: the box auto-grows, and a manual drag overrides that height.

## States

Focus uses the shared `--focus-*` ring. `:disabled` drops to `--opacity-disabled`. Hover excludes `:disabled` and `:read-only`, and is guarded by `@media (hover: hover) and (pointer: fine)`.

Like every control in the layer, the border is [an inset box-shadow rather than a real border](../form/#the-border-is-an-inset-shadow), so a hover or focus colour change never shifts the text inside by a pixel.

`::placeholder` sets `opacity: 100%` explicitly, so Firefox does not lighten the token colour a second time.
