---
isIndex: false
title: Tables
description: Collapsed borders, token-driven cell padding, a tinted header, and the last row that drops its rule.
weight: 15
icon: table
---

`base/table.css` styles `table`, `th` and `td` directly. There is no `.table` class.

```css
table {
  border-collapse: collapse;
  font-size: var(--table-font-size);
  width: 100%;
}

th, td {
  border-block-end: var(--border-width-sm) var(--border-style-normal) var(--table-color-border);
  padding-block: var(--table-cell-padding-block);
  padding-inline: var(--table-cell-padding-inline);
  text-align: start;
  vertical-align: top;
}
```

| Token | Role |
| --- | --- |
| `--table-font-size` | Table text size, usually a step down from the body |
| `--table-color-border` | The horizontal rule under each row |
| `--table-cell-padding-block` | Vertical cell padding |
| `--table-cell-padding-inline` | Horizontal cell padding |
| `--table-color-background-header` | `thead th` background |

## Rules under rows, not around cells

Only `border-block-end` is declared, so the table reads as a set of rows rather than as a grid. `border-collapse: collapse` is what keeps adjacent rules to a single line rather than doubling them.

The last row then drops its rule, because a line under the final row and a line under the table are the same line drawn twice:

```css
tbody tr:last-child td,
tbody tr:last-child th {
  border-block-end: none;
}
```

## `text-align: start`, not `left`

`start` follows the writing direction, so the table holds in a right-to-left document. Override it per column when the content calls for it, typically on numbers:

```css
td:last-child {
  text-align: end;
}
```

`vertical-align: top` is the choice that makes rows of uneven height readable: cells align on their first line rather than floating in the middle of the tallest cell.

## Block children inside a cell

```css
> *:first-child { margin-block-start: 0; }
> *:last-child  { margin-block-end: 0; }
```

A cell holding a `<p>` or a `<ul>` would otherwise inherit that element's block margin and pad itself twice. Trimming the first and last child leaves the cell padding in charge, while the spacing between several blocks in the same cell is preserved.

## Overflow

A wide table is not made scrollable here, because a scroll container has to be an element the table sits inside. Wrap it yourself when the content is wide:

```css
.table-scroll {
  overflow-x: auto;
}
```
