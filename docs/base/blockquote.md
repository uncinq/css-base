---
isIndex: false
title: Blockquotes
description: An inline-start border, italic body text, and a muted cite line set as a block.
weight: 7
icon: quote
---

`base/blockquote.css` styles `blockquote` and the `cite` inside it.

```css
blockquote {
  border-inline-start: var(--border-width-lg) var(--border-style-normal) var(--color-border);
  font-style: italic;
  margin-block: var(--spacing-typography);
  padding-inline-start: var(--spacing-sm);
}

blockquote cite {
  display: block;
  font-size: var(--font-size-xs);
  font-style: normal;
  margin-block-start: var(--spacing-typography);
}
```

```html
<blockquote>
  <p>The best way to predict the future is to invent it.</p>
  <cite>Alan Kay</cite>
</blockquote>
```

## The border is inline-start, not left

`border-inline-start` follows the writing direction, so the rule holds unchanged in a right-to-left document. The same goes for `padding-inline-start`. Nothing in this layer uses physical `left` or `right` properties.

## `cite` is forced back to upright

The quote is italic, so a nested `cite`, which the user agent also italicises, would be indistinguishable from the quotation it attributes. Setting `font-style: normal` on it restores the contrast, and `display: block` puts the attribution on its own line rather than trailing the last sentence.

`--spacing-typography` is the same token [paragraphs](../paragraphs/) use for their block margin, so the gap between the quote and its attribution matches the surrounding rhythm.

## Overriding

The border reads three generic tokens rather than blockquote-scoped ones, so changing it for quotes alone means writing the rule:

```css
@layer base {
  blockquote {
    border-inline-start-color: var(--color-brand);
  }
}
```
