---
isIndex: false
title: Lists
description: Prose list indentation and item spacing, wrapped in :where() so a navigation list overrides it for free.
weight: 6
icon: list-ul
---

`base/list.css` styles `ul`, `ol` and `li` for a **prose context**: a list inside running text, with markers and indentation.

```css
:where(ul, ol) {
  padding-inline-start: var(--size-16);
}

:where(ul, ol) :where(ul, ol) {
  margin-block: var(--spacing-typography);
  padding-inline-start: var(--spacing-sm);
}

:where(li) {
  margin-block: var(--spacing-2xs);
}
```

| Rule | Effect |
| --- | --- |
| `:where(ul, ol)` | Indents the list so markers sit inside the text column |
| Nested list | Adds block spacing and a smaller indent, so depth reads without running off the page |
| `:where(li)` | A small gap between items, from `--spacing-2xs` |

Nested lists read `--spacing-typography`, the same token as [paragraphs](../paragraphs/), so a list interrupts the prose rhythm rather than inventing its own.

## Everything is wrapped in `:where()`

That is the point of the file. `:where()` contributes **zero specificity**, so any navigation or UI list overrides these rules with a single class and no escalation:

```css
.nav {
  list-style: none;
  padding-inline-start: 0;
}
```

One class beats the base rule. Without `:where()`, `.nav` and `ul` would have different specificities and the reset would need `!important` or a longer selector.

This is why the file makes no attempt to exclude navigation lists itself. It does not need to: a UI list is a list that has been given a class, and the class always wins.

## Markers

Bullet and numbering styles are left to the user agent. [`.list` in @uncinq/css-components](../../../css-components/content/list/) builds its dash marker on top of this file rather than replacing it, which is why that component works on `ol` without a variant class.
