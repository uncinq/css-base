---
isIndex: false
title: Paragraphs
description: Block spacing and a measure cap on p, and the two places that cap is deliberately removed.
weight: 4
icon: text-paragraph
---

`base/typography.css` styles `p` and nothing else.

```css
p {
  margin-block: var(--spacing-typography);
  max-width: var(--max-width-paragraph);
}
```

| Token | Role |
| --- | --- |
| `--spacing-typography` | The rhythm between blocks. [Blockquotes](../blockquote/) and [nested lists](../list/) read the same token, so prose keeps one spacing unit |
| `--max-width-paragraph` | The measure cap, in `ch` or `rem`, so a paragraph wraps before it becomes hard to read |

## The measure cap is opt-out, not opt-in

`--max-width-paragraph` applies to every `p` on the page, including ones inside a layout that already constrains its width. Two files in this layer undo it where it would be wrong:

- [`base/address.css`](../address/) sets `margin-block: 0` and `text-wrap: unset` on its inner paragraphs, because an address is a block of short lines rather than prose.
- [`base/figure.css`](../figure/) sets `margin-block: 0` on paragraphs inside a figure, since the figure owns the gap between its own children.

Do the same in your own components when a paragraph is a layout element rather than a piece of running text:

```css
.card p {
  max-width: none;
}
```

## Wrapping

`text-wrap: pretty` and `overflow-wrap: break-word` come from the [reset](../../reset/). Pretty wrapping is what prevents a single-word last line, and it costs nothing on short paragraphs.
