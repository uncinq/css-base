---
isIndex: false
title: Accessibility helpers
description: .visually-hidden and .visually-hidden-focusable, the only two classes in @layer base.
weight: 24
icon: universal-access
---

`base/accessibility.css` is the only file in this layer that ships classes rather than element selectors.

| Class | Behaviour |
| --- | --- |
| `.visually-hidden` | Hidden visually, still exposed to assistive technology |
| `.visually-hidden-focusable` | The same, but revealed on `:focus` or `:focus-within`. Intended for skip links |

```css
.visually-hidden,
.visually-hidden-focusable:not(:focus):not(:focus-within) {
  clip: rect(0 0 0 0);
  clip-path: inset(50%);
  height: 1px;
  overflow: hidden;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}
```

## Why not `display: none`

`display: none` and `visibility: hidden` remove the element from the accessibility tree as well as from the page, so a screen reader never reaches it. These declarations shrink the element to a single clipped pixel instead: it stays in the tree, stays focusable, and is announced normally.

`white-space: nowrap` is the least obvious of the seven. Without it, text clipped into a 1px box wraps one character per line, and some screen readers then read it letter by letter.

`clip` is deprecated but kept alongside `clip-path` for older engines. Neither is redundant yet.

## Naming an icon-only control

```html
<button>
  <svg aria-hidden="true">...</svg>
  <span class="visually-hidden">Close</span>
</button>
```

`aria-hidden="true"` on the graphic and a hidden label beside it. `aria-label` on the button does the same job in one attribute; the hidden span is the better choice when the text has to be translatable through the same pipeline as the rest of the page.

No button in [@uncinq/css-components](../../../css-components/buttons/) supplies its own name, so this is the helper they expect.

## Skip links

`.visually-hidden-focusable` is what makes a skip link work: invisible until a keyboard user reaches it, visible the moment it takes focus.

```html
<a class="visually-hidden-focusable" href="#main">Skip to content</a>
```

`:focus-within` is in the selector as well as `:focus`, so a wrapper holding several links reveals itself when any one of them is focused. [`.nav-accessibility` in @uncinq/css-components](../../../css-components/navigation/nav-accessibility/) builds on exactly that.

## Do not use these to hide decorative content

Decorative content should be removed from the accessibility tree with `aria-hidden="true"`, which is the opposite trade-off: visible, not announced. These classes are announced, not visible. Using one where the other belongs either clutters the screen reader output with decoration or hides content from sighted users for no reason.
