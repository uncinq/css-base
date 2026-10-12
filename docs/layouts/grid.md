---
isIndex: false
title: Grid
description: A CSS grid whose column count follows the breakpoint, and the inherited --grid-column that lets one child span differently.
weight: 3
icon: grid-3x3
---

A CSS grid wrapper whose column count follows the breakpoint.

```html
<div class="grid">
  <div>...</div>
  <div>...</div>
</div>
```

```css
.grid {
  --columns: var(--columns-mobile, 1);
  display: grid;
  grid-template-columns: repeat(var(--columns), minmax(0, 1fr));
  gap: var(--grid-gap, var(--gap));

  & > * {
    grid-column: var(--grid-column);
  }
}
```

| Property read | Role |
| --- | --- |
| `--columns-mobile` … `--columns-desktop` | Column count per rung, falls back to `1` on mobile |
| `--grid-column-tablet` … `--grid-column-desktop` | The span each child takes, per rung |
| `--grid-gap` | Gap, falls back to `--gap` |

At each breakpoint the grid reassigns both `--columns` and `--grid-column`, so the track count and the default span move together.

## Tracks never grow past the container

The tracks are `minmax(0, 1fr)` rather than a bare `1fr`. A `1fr` track has an `auto` minimum: it cannot shrink below the min-content width of its widest child. One long unbreakable line in a `<pre>`, or a wide table, would then stretch the track past the container and make the whole page scroll sideways.

With a `0` minimum the tracks stay equal and the overflow stays inside the child, which handles it itself (`pre` already has `overflow-x: auto`). Give any other wide content its own scroll container:

```css
.table-wrapper {
  overflow-x: auto;
}
```

## Giving one child a different span

Children inherit `--grid-column` and apply it as their `grid-column`. To give one child a different span from its siblings, override the per-breakpoint property on that child:

```css
.featured {
  --grid-column-tablet: span 2;
  --grid-column-laptop: span 3;
}
```

Note that it is the **per-rung** token you override, not `--grid-column` itself. The grid reassigns `--grid-column` inside each media query, so a value set directly on the child is overwritten at the first breakpoint.

There is no `--grid-column-mobile`: below the first breakpoint the grid is one column and every child fills it.

## The gap has its own token

```css
gap: var(--grid-gap, var(--gap));
```

`--gap` is the global spacing between layout children. `--grid-gap` overrides it for one grid without touching the global value:

```css
.tight {
  --grid-gap: var(--spacing-xs);
}
```

[`.row`](../row/) reads the same pair, which is what keeps a row and a grid gutter-aligned, and its `.col-*` width formula subtracts the same value.

## When to use `.grid` rather than `.row`

`.grid` places children on tracks: every cell is one column wide by default and the rows are implicit. It is the right choice for a set of equal cells, such as a list of cards, and it is what [`.items` in @uncinq/css-components](../../../css-components/content/items/) builds on.

Use [`.row`](../row/) when the children take different proportions of the width through `.col-*` modifiers. Both resolve against the same `--columns`, so the two line up when placed side by side.
