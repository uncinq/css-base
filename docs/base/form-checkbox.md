---
isIndex: false
title: Checkboxes
description: A custom checkbox built from appearance none, with a masked tick, an indeterminate state and a clickable adjacent label.
weight: 18
icon: check-square
---

`base/form-checkbox.css` replaces the native checkbox with a box this package draws itself. Largely inspired by [KNACSS](https://knacss.com/).

```css
[type='checkbox']:not([role='switch']) { … }
```

The `:not([role='switch'])` is what keeps this file and [the switch](../form-switch/) apart: a switch is a checkbox with a role, and it needs a different shape entirely.

## The box

| Token | Role |
| --- | --- |
| `--checkable-size` | Width and height, falls back to `1.25rem`. Shared with [radio](../form-radio/) and [switch](../form-switch/) |
| `--checkbox-color-background` | Unchecked background |
| `--checkbox-color-background-checked` | Checked background |
| `--checkbox-color-border` | Border colour, drawn as an inset shadow |
| `--checkbox-color-border-checked` | Checked border colour |
| `--checkbox-color-border-hover` | Hover border colour |
| `--checkbox-border-width` | Border thickness |
| `--checkbox-border-radius` | Corner radius. Set it to a pill value and the checkbox reads as a round one |
| `--checkbox-mask-checked` | The tick glyph |
| `--checkbox-mask-indeterminate` | The dash glyph |
| `--checkbox-scale-checked` | Tick size inside the box, falls back to `0.7` |
| `--form-color-accent` | Tick colour |

`flex: 0 0 auto` matters more than it looks: in a flex row next to a long label, a checkbox without it is compressed into an ellipse.

## The tick is a mask

```css
&:checked::after {
  background-color: var(--form-color-accent, #000000);
  mask: var(--checkbox-mask-checked);
  scale: var(--checkbox-scale-checked, 0.7);
}
```

A mask over a colour rather than a background image, so the tick takes the accent token and stays crisp at any size. `--form-color-accent` is the same token the [file selector button](../form/#per-type-handling) uses.

## The indeterminate state is real

```css
&:indeterminate::after {
  background-color: currentcolor;
  mask: var(--checkbox-mask-indeterminate);
}
```

`:indeterminate` is set from JavaScript, never from markup:

```js
checkbox.indeterminate = true;
```

It is the correct state for a "select all" box whose children are partly checked. Note that the background treats it like a checked box, through `&:is(:checked, :indeterminate)`, while the glyph is the dash rather than the tick.

## The adjacent label

```css
[type='checkbox'] + label {
  --label-padding: var(--checkbox-label-padding);
}

[type='checkbox']:not(:disabled) + label {
  cursor: pointer;
}
```

The pointer cursor is applied only when the control is enabled, so a disabled row does not invite a click that does nothing. Both rules need the label to be the **immediately following sibling**, which is the markup [`.form-check` in @uncinq/css-components](../../../css-components/forms/form-check/) lays out:

```html
<div class="form-check">
  <input type="checkbox" id="terms">
  <label for="terms">I accept the terms</label>
</div>
```

Wrapping the input inside the label instead is valid HTML but breaks both selectors.

## States

Focus uses the shared `--focus-*` ring. `:disabled` drops to `--opacity-disabled`. Hover excludes `:disabled` and `:read-only` and is guarded by `@media (hover: hover) and (pointer: fine)`.
