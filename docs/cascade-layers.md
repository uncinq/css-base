---
isIndex: false
title: Cascade layers
description: The three layers this package owns, and why declaring the full order is the consuming project's job.
weight: 1
---

css-base scopes every rule it ships to one of three cascade layers it owns.

| Layer | Contents |
| --- | --- |
| `@layer reset` | The CSS reset, the foundational baseline |
| `@layer base` | Native element styles |
| `@layer layouts` | Layout primitives |

Custom media queries in `css/mediaqueries.css` are the exception: `@custom-media` is resolved at build time, so it is never subject to the cascade and sits outside any layer.

## Declare the order in your project

The full layer order, including layers owned by sibling packages (`tokens`, `components`) and by the project itself (`libs`, `vendors`, `pages`, `utilities`), belongs to the **consuming project**. css-base deliberately does not declare it.

The reason is a rule of the cascade: a layer's position is fixed the **first time its name is seen**, and later re-declarations do not reorder it. If an imported package declared the order, it would win, and your project could never change it.

So declare the order once, at the very top of your entry stylesheet, before any `@import`:

```css
/* your project entry, e.g. main.css */
@layer reset, tokens, libs, vendors, base, layouts, components, pages, utilities;

@import '@uncinq/design-tokens';    /* @layer tokens */
@import '@uncinq/css-base';         /* @layer reset, base, layouts */
@import '@uncinq/component-tokens'; /* @layer tokens */
@import '@uncinq/css-components';   /* @layer components */
```

The last layer wins. `reset` and `tokens` come first, at the lowest priority, so reset rules and token defaults never accidentally override base, layout, vendor or component styles.

If the `@layer ...;` line comes *after* an import, it is already too late: the imported package has fixed the order. Keep it first, always.

## Why `libs` and `vendors` are a pair

A third-party stylesheet goes into `libs`; the CSS you write to dress it goes into `vendors`, immediately after. Keeping them separate matters whenever the library is loaded lazily: it then arrives after your own CSS, and inside a single shared layer it would win every tie on source order.

Declare `libs` even when nothing imports into it yet. An undeclared layer name is appended **last**, above `utilities`, and CSS outside any layer is stronger still. A library imported with no `layer()` at all would therefore beat everything you write.

## Overriding from your project

Write to the same layer name after the imports:

```css
@layer base {
  a {
    --color-link: #0070f3;
  }
}
```

Because css-base uses low-specificity selectors throughout, and `:where()` in several files, an override rarely needs to escalate specificity. Prefer redefining the custom property, as above, over rewriting the declaration.

## Reference

[MDN: Using CSS cascade layers](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)
