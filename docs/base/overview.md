---
isIndex: false
title: Overview
description: What @layer base covers, how the 23 files are grouped, and the two conventions that run through all of them.
weight: 1
icon: book
---

`@layer base` styles native HTML elements so that unstyled markup already looks right. There is no class to apply: write `<table>`, `<blockquote>` or `<input type="date">` and they are styled.

Every value comes from a custom property resolved by [@uncinq/design-tokens](https://github.com/uncinq/design-tokens) and [@uncinq/component-tokens](https://github.com/uncinq/component-tokens). This package hardcodes almost nothing, so restyling is a matter of changing a token, not overriding a rule.

`css/base.css` imports all 23 files below, one page each.

## Typography and text

| Page | File | Selectors |
| --- | --- | --- |
| [Body](../body/) | `base/body.css` | `body`: background, text color, font family, size, weight, line-height |
| [Headings](../headings/) | `base/headings.css` | `h1` to `h6`, plus a per-level size from `--font-size-heading-01` to `-06` |
| [Paragraphs](../paragraphs/) | `base/typography.css` | `p`: block spacing and `--max-width-paragraph` |
| [Links](../link/) | `base/link.css` | `a`: colour, underline, focus ring, external-link marker |
| [Lists](../list/) | `base/list.css` | `ul`, `ol`, `li` in prose context, including nested indentation |
| [Blockquotes](../blockquote/) | `base/blockquote.css` | `blockquote`, `blockquote cite` |
| [Code](../code/) | `base/code.css` | `code`, `kbd`, `samp`, `pre`, and inline `code` separately from block code |
| [Address](../address/) | `base/address.css` | `address`: removes the UA italic and resets inner paragraph spacing |
| [Abbreviations](../abbr/) | `base/abbr.css` | `abbr[title]`: dotted underline with its own offset and thickness |
| [Superscript](../sup/) | `base/sup.css` | `sup`: raised without growing the line box |

## Media and embedded content

| Page | File | Selectors |
| --- | --- | --- |
| [Figures](../figure/) | `base/figure.css` | `figure`, `figcaption`, and nested `picture`, `img`, `.credit` |
| [Picture](../picture/) | `base/picture.css` | `picture`: `inline-block` |
| [Video](../video/) | `base/video.css` | `video`: fluid width, auto height |
| [Tables](../table/) | `base/table.css` | `table`, `thead`, `tbody`, `th`, `td` |
| [Details](../details/) | `base/details.css` | `details`, `summary`, `details[open]`, the marker and the disclosed panel |

## Forms

Six of these seven files are largely inspired by [KNACSS](https://knacss.com/).

| Page | File | Selectors |
| --- | --- | --- |
| [Inputs and labels](../form/) | `base/form.css` | `label`, `legend`, and `input` excluding button, reset, submit, checkbox, radio and range |
| [Checkboxes](../form-checkbox/) | `base/form-checkbox.css` | `[type='checkbox']` when it is not a switch, plus its adjacent `label` |
| [Radios](../form-radio/) | `base/form-radio.css` | `[type='radio']` plus its adjacent `label` |
| [Range](../form-range/) | `base/form-range.css` | `[type='range']`, the `.range` wrapper and the `.range-value` output |
| [Select](../form-select/) | `base/form-select.css` | `select` and `optgroup`, with a token-driven chevron |
| [Switch](../form-switch/) | `base/form-switch.css` | `[role="switch"]`, rendered as a track and knob |
| [Textarea](../form-textarea/) | `base/form-textarea.css` | `textarea`, including `field-sizing: content` |

Text-like controls (`input`, `textarea`, `select`, `checkbox`, `radio`) draw their outline with `box-shadow: inset` and `border: 0`, so that focus and hover states change colour without shifting layout by a pixel. The [switch](../form-switch/) and the [range](../form-range/) thumb are the exceptions: both need a real `border`, since their outline is part of a shape that moves.

## Accessibility helpers

| Page | File | Ships |
| --- | --- | --- |
| [Accessibility helpers](../accessibility/) | `base/accessibility.css` | `.visually-hidden`, `.visually-hidden-focusable` |

It is the only file in this layer that ships classes rather than element selectors.

## Two conventions run through every file

**Hover is guarded.** Every hover style in this layer sits inside `@media (hover: hover) and (pointer: fine)`, so nothing sticks on touch.

**Motion is guarded.** Every transition sits inside `@media (prefers-reduced-motion: no-preference)`.

## Overriding

Redefine the token rather than the rule:

```css
@layer base {
  :root {
    --table-cell-padding-block: 0.5rem;
  }
}
```

Several files read a component-scoped property with a semantic fallback, for example `var(--code-font-family, var(--font-family-code))`. That gives you two levels: change the component token to affect only that element, or the semantic token to affect everything that inherits from it.
