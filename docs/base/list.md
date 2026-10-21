---
isIndex: false
title: Lists
description: Prose list indentation in em, sized for the marker, and item spacing, wrapped in :where() so a navigation list overrides it for free.
weight: 6
icon: list-ul
---

`base/list.css` styles `ul`, `ol` and `li` for a **prose context**: a list inside running text, with markers and indentation.

```css
:where(ul) {
  list-style-type: var(--list-style-type, revert-layer);
  padding-inline-start: var(--list-padding-inline-start, 1.5em);
}

:where(ol) {
  list-style-type: var(--list-ordered-style-type, revert-layer);
  padding-inline-start: var(--list-ordered-padding-inline-start, 2em);
}

:where(ul, ol) :where(ul, ol) {
  margin-block-end: var(--list-nested-margin-block-end, var(--spacing-typography));
  margin-block-start: var(--list-nested-margin-block-start, var(--spacing-typography));
}

:where(li) {
  margin-block: var(--list-item-gap, var(--spacing-2xs));
}
```

| Rule | Effect |
| --- | --- |
| `:where(ul)` | Indents the list so the bullets sit inside it, from `--list-padding-inline-start`, and takes its marker from `--list-style-type` when set |
| `:where(ol)` | The same, with room for two-digit numbers, from `--list-ordered-padding-inline-start` and `--list-ordered-style-type` |
| Nested list | Block spacing above and below a list inside an item, from `--list-nested-margin-block-start` and `--list-nested-margin-block-end` |
| `:where(li)` | A small gap between items, from `--list-item-gap` |

Nested lists get no indent rule, see [nested lists](#nested-lists).

## Markers stay outside

The file keeps the user agent's `list-style-position: outside`: the marker sits in the list's padding, before the item text. `inside` looks safer, since the marker can never leave the box, but it costs more than it saves:

| Case | `outside` | `inside` |
| --- | --- | --- |
| A wrapped line | Starts under the text | Starts under the marker |
| `<li><p>`, a loose Markdown list | Marker on the first line of the paragraph | Marker alone on a line above the paragraph |
| Item 10 after item 9 | Numbers right-aligned, text aligned | Text pushed one digit further |
| A nested list | Starts at the parent item's text | Starts under the parent marker, needs its own offset rule |
| A marker wider than the indent | Overflows the box | Stays inside |

The `<li><p>` row settles it. Markdown renderers, Hugo's Goldmark included, wrap every item in a `p` as soon as the items are separated by a blank line, and render every footnote that way. Chrome, Firefox and Safari all put an inside marker on its own line in front of that paragraph.

The last row is the only advantage of `inside`, and an indent sized in em removes the overflow at its source. `inside` remains the right call for a centered list, where an outside marker would stay at the start edge, or for items laid out in a row, where it would hang into the previous item. Set it on that list's class.

## The indent is in em

A marker is text, so it grows with the font size of the list. An indent in `rem` does not: once a list is set larger than the root size, its markers spill out of the box, and an ancestor with `overflow: hidden` clips them. In em, the indent scales with the marker.

The defaults come from measuring outside markers with Roboto, Playfair Display and system-ui, from 14px to 24px, in Chrome, Firefox and Safari's engine. Safari is the widest, because it adds a gap in pixels after the marker, so the room needed in em grows as the text gets smaller:

| Marker | Room needed, Safari at 14px | Default |
| --- | --- | --- |
| Bullet | about 1.3em | `--list-padding-inline-start: 1.5em` |
| `1.` to `99.` | about 2em | `--list-ordered-padding-inline-start: 2em` |
| `100.` and above | about 2.5em | Raise the indent of that list |

These figures hold for fonts with ordinary figures. Wider ones need more: Verdana takes about 2.3em for `10.` in Safari, so a theme set in such a font should raise `--list-ordered-padding-inline-start`.

## Nested lists

**Indent.** A nested list starts at the text edge of its parent item, and its own padding adds the next step. Every level indents by one step without a dedicated selector, a nested `ol` inside a `ul` takes the ordered indent, and a nested list set in a smaller font gets a proportionally smaller step.

**Spacing.** A nested list takes `--list-nested-margin-block-start` above it and `--list-nested-margin-block-end` below, both `--spacing-typography` by default, the same token as [paragraphs](../paragraphs/), so a sub-list interrupts the prose rhythm rather than inventing its own. That is more than the gap between items, on both sides, so the sub-list sits as far from its parent item as from the next one. The two tokens are separate so that the sides can differ: set the start one to `var(--list-item-gap)` and the sub-list sticks to its parent item while the next item still stands apart. Set both to it and the margins collapse into the item gap. In a loose list the paragraph margins already match the default, so items and sub-lists end up evenly spaced.

**A flat outer list.** Because every list carries its own indent, removing it from the outer list leaves the nested ones alone. A list stripped of its markers keeps indented, marked sub-lists:

```css
.plain {
  list-style: none;
  padding-inline-start: 0;
}
```

Remove it with the property, as above, and not with the token: set on `.plain`, `--list-padding-inline-start: 0` would be inherited by every list inside it, and their markers would end up outside the box.

## Overriding the tokens

The five tokens come from [@uncinq/component-tokens](../../../component-tokens/reference/): the two indents, the two nested margins, and `--list-item-gap`, the item spacing the [labelled list](../../../css-components/content/list/) reads as well, so both kinds of list keep the same rhythm. Each `var()` carries a fallback for a token package that predates them: the default value for the indents, the semantic spacing token for the margins. The two marker hooks are not tokens, see [Markers](#markers).

They inherit like any custom property, so the element you set them on is the scope they change, nested lists included. That makes them the tool for resizing every list in a scope; to change one list alone, set the property on it:

```css
/* Every list on the page */
:root {
  --list-padding-inline-start: 1.25em;
}

/* One long numbered list, its nested lists untouched */
.changelog {
  padding-inline-start: 2.5em;
}
```

An indent is resolved on each list, against that list's font size, so keep it in em: a `rem` or `px` value brings the overflow back at large sizes. A theme that sets the markers bolder or larger than the text should raise both indents with them.

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

Removing the markers has an accessibility cost in Safari. WebKit treats a list without visible markers as a layout list, and VoiceOver on iOS then reads its items without announcing a list or its length. Where that information matters, a list of results or of cards for instance, add `role="list"` to the element: it looks redundant, and validators flag it, but it is the documented way to keep the role.

## Markers

The marker type is a hook rather than a token: `--list-style-type` for `ul`, `--list-ordered-style-type` for `ol`. Neither is defined by default, and the `revert-layer` fallback hands the property back to the user agent, so a page gets the browser's own markers: disc, then circle and square for nested `ul`, decimal for `ol`. A default value in component-tokens would have flattened that progression to a single marker, which is why the two stay undefined, like `--nav-list-style` in css-components.

Set one to change every list in a scope, nested levels included:

```css
:root {
  --list-style-type: "– ";
}
```

Any `list-style-type` value works: a keyword, a string like the dash above, a `@counter-style` name, or `none`, which has the accessibility cost described in [the previous section](#everything-is-wrapped-in-where). The marker stays outside, for the reasons given [at the top](#markers-stay-outside). The file sets no `::marker` rule, and [`.list` in @uncinq/css-components](../../../css-components/content/list/) adds none either, which is why that component works on `ol` without a variant class.
