---
isIndex: false
title: Base
description: Styles for native HTML elements, one file per element or element family, all in @layer base.
weight: 3
icon: type
---

`@layer base` styles native HTML elements so that unstyled markup already looks right. There is no class to apply: write `<table>`, `<blockquote>` or `<input type="date">` and they are styled.

Every value comes from a custom property resolved by [@uncinq/design-tokens](https://github.com/uncinq/design-tokens) and [@uncinq/component-tokens](https://github.com/uncinq/component-tokens). This package hardcodes almost nothing, so restyling is a matter of changing a token, not overriding a rule.

`css/base.css` imports all 23 files below.

## Typography and text

| File | Selectors |
| --- | --- |
| `base/body.css` | `body`: background, text color, font family, size, weight, line-height |
| `base/headings.css` | `h1` to `h6`: color, font family, weight, letter-spacing, line-height, margin, plus a per-level size from `--font-size-heading-01` to `-06` |
| `base/typography.css` | `p`: block spacing and `--max-width-paragraph` |
| `base/link.css` | `a`: color, underline offset, thickness and color, hover transition, focus-visible outline |
| `base/blockquote.css` | `blockquote`, `blockquote cite`: inline-start border, italic, muted cite block |
| `base/list.css` | `ul`, `ol`, `li` in prose context, including nested list indentation |
| `base/code.css` | `code`, `kbd`, `samp`, `pre`, and inline `code` outside `pre` styled separately from block code |
| `base/address.css` | `address`: removes the UA italic and resets inner paragraph spacing |
| `base/abbr.css` | `abbr[title]`: dotted underline with its own offset and thickness |
| `base/sup.css` | `sup`: raised without growing the line box, see the note below |

## Media and embedded content

| File | Selectors |
| --- | --- |
| `base/figure.css` | `figure`, `figcaption`, and nested `picture`, `img`, `.credit`. Flex column layout with responsive caption direction |
| `base/picture.css` | `picture`: `inline-block` |
| `base/video.css` | `video`: fluid width, auto height |
| `base/table.css` | `table`, `thead`, `tbody`, `th`, `td`: collapsed borders, cell padding, header weight, no border on the last row |
| `base/details.css` | `details`, `summary`, `details[open]`, the `summary::after` marker and the disclosed panel |

## Forms

Six of these seven files are largely inspired by [KNACSS](https://knacss.com/).

| File | Selectors |
| --- | --- |
| `base/form.css` | `label`, `legend`, and `input` excluding button, reset, submit, checkbox, radio and range. Adds per-type handling for `search`, `number`, `date`, `time`, `datetime-local` and `file`, including the `::file-selector-button` |
| `base/form-checkbox.css` | `[type='checkbox']` when it is not a switch, plus its adjacent `label` |
| `base/form-radio.css` | `[type='radio']` plus its adjacent `label` |
| `base/form-range.css` | `[type='range']`, the `.range` wrapper, `.range-value` output, and the WebKit and Gecko track and thumb pseudo-elements |
| `base/form-select.css` | `select` and `optgroup`, with a token-driven chevron background image |
| `base/form-switch.css` | `[role="switch"]`, rendered as a track and knob |
| `base/form-textarea.css` | `textarea`, including `field-sizing: content` and a `--textarea-min-height` default of `5lh` |

Text-like controls (`input`, `textarea`, `select`, `checkbox`, `radio`) draw their outline with `box-shadow: inset` and `border: 0`, so that focus and hover states change color without shifting layout by a pixel. The switch and the range thumb are the exceptions: both need a real `border`, since their outline is part of a shape that moves.

Every hover style in this layer is guarded by `@media (hover: hover) and (pointer: fine)`, so nothing sticks on touch. Transitions are guarded by `@media (prefers-reduced-motion: no-preference)`.

## Accessibility helpers

`base/accessibility.css` is the only file in this layer that ships classes rather than element selectors.

| Class | Behaviour |
| --- | --- |
| `.visually-hidden` | Hidden visually, still exposed to assistive technology |
| `.visually-hidden-focusable` | The same, but revealed on `:focus` or `:focus-within`. Intended for skip links |

```html
<button>
  <svg aria-hidden="true">...</svg>
  <span class="visually-hidden">Close</span>
</button>
```

Do not use these to hide decorative content. Decorative content should be removed from the accessibility tree with `aria-hidden="true"` instead, which is the opposite trade-off.

## Why `sup` zeroes its line-height

The user agent default, `vertical-align: super`, raises the glyph inside the line box, so the line box grows. A block containing a `<sup>` then becomes taller than one without it.

Zeroing `line-height` takes the superscript out of the line box calculation, and relative positioning raises it without affecting layout. A label carrying a required marker then measures exactly like a label without one, and two fields side by side stay aligned.

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
