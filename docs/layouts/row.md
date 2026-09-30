---
isIndex: false
title: Row
description: The flex wrapper, its four proportional column modifiers, the gap term in their width formula, and the two offsets.
weight: 4
icon: columns-gap
---

A flex wrapper, stacked on mobile and turning into a row at the `--tablet` breakpoint and above.

```html
<div class="row">
  <div class="col-small">...</div>
  <div class="col-small">...</div>
</div>
```

```css
.row {
  --columns: var(--columns-mobile, 1);
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  gap: var(--grid-gap, var(--gap));
}
```

`.row` mirrors the [grid](../grid/)'s responsive column count into its own `--columns`, so `.col-*` resolves against the same tracks as `.grid`. A row and a grid placed side by side line up.

## Column modifiers

Its column modifiers apply to direct children. Each one spans a share of the active column count, so the proportion holds whatever that count is, and lands on whole columns whenever the count is divisible, such as 12, 24 or 48.

| Modifier | Span | At 12 columns |
| --- | --- | --- |
| `.col-xsmall` | `cols / 3` | 1/3 |
| `.col-small` | `cols / 2` | 1/2 |
| `.col-medium` | `cols * 2 / 3` | 2/3 |
| `.col-large` | `cols * 5 / 6` | 5/6 |

They are declared **inside** `@media (--tablet)`, alongside `flex-direction: row`. Below that width every child is full width and the modifier does nothing, which is what makes the row stack on mobile with no extra class.

## Why the width formula has a gap term

```css
flex: 0 0 calc(
  var(--col-span) / var(--columns) * 100%
  - (1 - var(--col-span) / var(--columns)) * var(--grid-gap, var(--gap))
);
```

The computed width is `span/cols * 100% - (1 - span/cols) * gap`. The second term gives back the gaps the item does not occupy, which is what keeps the proportions exact rather than approximate.

Without it, two `.col-small` children would each be exactly 50% and the gap between them would push the pair past 100%, wrapping the second onto its own line. With it, two halves plus one gap come to exactly the row width, and a half in a row stays aligned with six columns of a twelve-column [grid](../grid/).

## Offsets

Two offset modifiers are available, from the `--tablet` breakpoint (768px) upwards, alongside the column modifiers:

| Modifier | Effect |
| --- | --- |
| `.offset-center` | `margin-inline: auto` |
| `.offset-end` | `margin-inline-start: auto` |

```html
<div class="row">
  <div class="col-small offset-center">...</div>
</div>
```

Both are inline-direction aware, so they follow a right-to-left document without a variant.

## `flex-wrap: wrap` is deliberate

Children whose spans add up to more than the row wrap onto a new line rather than being compressed below their declared proportion. A `.col-medium` and a `.col-small` in the same row, which is 2/3 plus 1/2, wrap on purpose.
