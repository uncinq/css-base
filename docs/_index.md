---
isIndex: false
title: css-base
description: Framework-agnostic CSS foundation, providing a reset, native element styles and layout primitives.
weight: 4
icon: box
---

`@uncinq/css-base` is the CSS foundation the other Un Cinq packages build on. It styles native HTML elements and provides a handful of layout primitives, without introducing a component system and without any framework dependency.

It ships plain CSS source files. There is no build step, no preprocessor and no JavaScript.

## What is included

| Group | Scope |
| --- | --- |
| [Reset](reset/) | Baseline normalization, based on the Josh W Comeau custom CSS reset |
| [Base](base/) | Styles for native HTML elements, driven entirely by design tokens |
| [Layouts](layouts/) | `.container`, `.grid`, `.row` and their modifiers |
| [Media queries](mediaqueries/) | The `--sm` / `--md` / `--lg` / `--xl` custom media scale |

Each group scopes itself to a cascade layer it owns. Read [Cascade layers](cascade-layers/) before your first import: the layer order is the consuming project's responsibility, and getting it wrong is the most common source of surprises.

## Installation

```bash
npm install @uncinq/css-base
```

This package carries no token values of its own. It resolves every custom property it references from two peer dependencies, which must be installed and imported first:

```bash
npm install @uncinq/design-tokens @uncinq/component-tokens
```

## Usage

Import everything at once:

```css
@import '@uncinq/css-base';
```

Import a single group:

```css
@import '@uncinq/css-base/css/reset.css';
@import '@uncinq/css-base/css/base.css';
@import '@uncinq/css-base/css/layouts.css';
```

Or import file by file, when you want only part of the base layer:

```css
@import '@uncinq/css-base/css/base/body.css';
@import '@uncinq/css-base/css/base/headings.css';
@import '@uncinq/css-base/css/layouts/container.css';
```

The full import order, tokens before CSS, looks like this:

```css
@layer reset, tokens, libs, vendors, base, layouts, components, pages, utilities;

@import '@uncinq/design-tokens';    /* @layer tokens */
@import '@uncinq/css-base';         /* @layer reset, base, layouts */
@import '@uncinq/component-tokens'; /* @layer tokens */
@import '@uncinq/css-components';   /* @layer components */
```

## Build requirement

`css/mediaqueries.css` declares `@custom-media` rules, which no browser implements natively. Your build must run [postcss-custom-media](https://www.npmjs.com/package/postcss-custom-media), otherwise every `@media (--sm)` query in the layout files is dropped and the layouts stay at their mobile values. See [Media queries](mediaqueries/).

## File structure

```
css/
  index.css           imports mediaqueries, reset, base, layouts
  mediaqueries.css    @custom-media scale, outside any layer
  reset.css           @layer reset
  base.css            barrel, imports all base/*
  layouts.css         barrel, imports all layouts/*
  base/               23 files, @layer base
  layouts/            3 files, @layer layouts
```

## References

- [@uncinq/design-tokens](https://github.com/uncinq/design-tokens), primitive and semantic tokens
- [@uncinq/component-tokens](https://github.com/uncinq/component-tokens), component-scoped tokens
- [@uncinq/css-components](https://github.com/uncinq/css-components), the component layer built on this package
