---
isIndex: false
title: Radios
description: The same box as the checkbox, drawn round, with a ::before dot that scales in from zero.
weight: 19
icon: record-circle
---

`base/form-radio.css` mirrors [the checkbox](../form-checkbox/) almost rule for rule, with one structural difference: the dot is a real element that scales, not a mask that appears. Largely inspired by [KNACSS](https://knacss.com/).

```css
[type='radio'] {
  appearance: none;
  border-radius: var(--radio-border-radius);
  box-shadow: inset 0 0 0 var(--radio-border-width) var(--radio-color-border);
  display: inline-grid;
  height: var(--checkable-size, 1.25em);
  place-content: center;
  width: var(--checkable-size, 1.25em);
}
```

| Token | Role |
| --- | --- |
| `--checkable-size` | Width and height, falls back to `1.25em`. Shared with [checkbox](../form-checkbox/) and [switch](../form-switch/) |
| `--radio-color-background` | Track background |
| `--radio-border-radius` | Corner radius, a pill value in practice |
| `--radio-border-width` | Border thickness, drawn as an inset shadow |
| `--radio-color-border` | Border colour |
| `--radio-color-border-hover` | Hover border colour |
| `--radio-color-background-checked` | Dot fill |
| `--radio-color-border-checked` | Dot border |
| `--radio-border-width-checked` | Dot border thickness |
| `--radio-scale-checked` | Dot size when checked, falls back to `0.7` |

## The dot is always there

```css
&::before {
  border-radius: var(--radius-pill, 9999px);
  content: '';
  scale: 0;
  /* … */
}

&:checked::before {
  scale: var(--radio-scale-checked, 0.7);
}
```

The `::before` exists at every moment; only its `scale` changes. That is what makes the transition possible: `scale` animates, while a `::before` toggled between `content: none` and `content: ''` would pop in.

`place-content: center` on the `inline-grid` parent keeps the dot centred at any scale, with no positioning maths.

## `em` rather than `rem`

`--checkable-size` falls back to `1.25em` here and to `1.25rem` for the [checkbox](../form-checkbox/). That is a real difference: a radio sized in `em` follows the font size of whatever surrounds it, so a radio in a small print block shrinks with its label. Set `--checkable-size` explicitly if you want both controls to match exactly.

## The adjacent label

```css
[type='radio'] + label {
  --label-padding: var(--radio-label-padding);
}

[type='radio']:not(:disabled) + label {
  cursor: pointer;
}
```

Same contract as the checkbox: the label must be the immediately following sibling.

## Grouping

Radios in the same group share a `name`. A group also needs a group label, which means a `<fieldset>` with a `<legend>`, styled alongside `label` in [`base/form.css`](../form/#labels-and-legends):

```html
<fieldset>
  <legend>Delivery</legend>
  <div class="form-check">
    <input type="radio" name="delivery" id="standard" value="standard">
    <label for="standard">Standard</label>
  </div>
  <div class="form-check">
    <input type="radio" name="delivery" id="express" value="express">
    <label for="express">Express</label>
  </div>
</fieldset>
```

Without the fieldset, a screen reader announces each option but never the question.
