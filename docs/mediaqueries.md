---
isIndex: false
title: Media queries
description: The --sm to --xl custom media scale, the deprecated device aliases, and the PostCSS plugin the package requires.
weight: 5
---

`css/mediaqueries.css` declares the breakpoint scale as `@custom-media` rules. It is imported first by `css/index.css` and sits outside any cascade layer, because `@custom-media` is resolved at build time and is never subject to the cascade.

## The scale

| Custom media | Min width | Pixels at 16px root |
| --- | --- | --- |
| `--sm` | `48rem` | 768 |
| `--md` | `64rem` | 1024 |
| `--lg` | `90rem` | 1440 |
| `--xl` | `100rem` | 1600 |

```css
.thing {
  @media (--md) {
    display: grid;
  }
}
```

These are rungs on a ladder, not devices. A width tells you nothing about the hardware: a 1024px window on a 27 inch monitor is not a tablet. Naming the steps after devices is also what left the previous scale with a compound name, `--tablet-wide`, which belongs to no device at all.

Intent belongs one layer up, in the semantic custom media a component defines for itself, such as `--toc-expand` or `--header-expand`. That way the component states the width at which *its own* behaviour changes, and the generic scale stays free of assumptions.

## Deprecated device aliases

The previous names are kept as aliases so existing stylesheets keep working.

| Deprecated | Use instead |
| --- | --- |
| `--tablet` | `--sm` |
| `--tablet-wide` | `--md` |
| `--laptop` | `--lg` |
| `--desktop` | `--xl` |

They are removed in the next major version. Prefer the scale in new code.

Note that the layout files shipped by this package, `container.css`, `grid.css` and `row.css`, still use the deprecated names internally. That is a migration still to be done, and it does not affect consumers: both sets of names resolve to the same widths.

The token names follow the same device vocabulary (`--container-max-width-tablet`, `--columns-laptop`), and those are token concerns rather than media query concerns. They are not affected by this deprecation.

## Required build step

No browser implements `@custom-media`. Your build must run [postcss-custom-media](https://www.npmjs.com/package/postcss-custom-media).

```bash
npm install --save-dev postcss postcss-custom-media
```

```js
// postcss.config.js
module.exports = {
  plugins: [require('postcss-custom-media')],
};
```

This failure is quiet rather than loud: without the plugin every `@media (--sm)` block is simply dropped, no error is raised, and the page renders at its mobile values on every screen size. If your layouts never respond to width, check this first.
