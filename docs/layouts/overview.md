---
isIndex: false
title: Overview
description: Why there are only three primitives, the column count they share, and the build step they all need.
weight: 1
icon: book
---

`@layer layouts` ships three primitives. They are deliberately few: everything else is a component's job.

| Page | Class | Role |
| --- | --- | --- |
| [Container](../container/) | `.container` | Centred wrapper with a responsive max-width and a gutter |
| [Grid](../grid/) | `.grid` | CSS grid whose column count follows the breakpoint |
| [Row](../row/) | `.row` | Flex wrapper with proportional column modifiers |

All three read the responsive column count and gutter from tokens, so a project changes its grid by changing tokens rather than by rewriting layout rules.

## `.grid` and `.row` share one column count

Both mirror the same `--columns-mobile` / `-tablet` / `-tablet-wide` / `-laptop` / `-desktop` tokens into their own `--columns`. A row and a grid placed side by side therefore resolve against the same tracks and line up, without either knowing about the other.

Pick between them on what the content needs: `.grid` for a set of equal cells, `.row` when children take different proportions through `.col-*`.

## The token names still say tablet

The layout files use `@media (--tablet)`, `@media (--laptop)` and their siblings, which are the [deprecated device aliases](../../mediaqueries/#deprecated-device-aliases) of the `--sm` to `--xl` scale. That is a migration still to be done inside the package, and it changes nothing for consumers: both sets of names resolve to the same widths.

The token names follow the same device vocabulary, `--container-max-width-tablet` and `--columns-laptop`, and those are token concerns rather than media query concerns. They are not affected by the deprecation.

## Build requirement

All three files use `@custom-media` rules. Without [postcss-custom-media](https://www.npmjs.com/package/postcss-custom-media) in your build, those blocks are dropped and the layouts stay at their mobile values: one column, no max-width steps, no rows. See [Media queries](../../mediaqueries/).

The failure is silent. Nothing errors, the page simply never responds to width.
