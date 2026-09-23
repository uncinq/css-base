---
isIndex: false
title: Layouts
description: The .container, .grid and .row primitives, and the --container-bleed contract.
weight: 4
---

`@layer layouts` ships three primitives. They are deliberately few: everything else is a component's job.

All three read the responsive column count and gutter from tokens, so a project changes its grid by changing tokens rather than by rewriting layout rules.

## `.container`

A centered wrapper with a responsive max-width and a gutter on both sides.

```html
<div class="container">...</div>
```

| Property read | Role |
| --- | --- |
| `--container-max-width-*` | Max-width per breakpoint rung |
| `--container-bleed-*` | Bleed allowance per rung, the paired token |
| `--gutter` | Inline padding |

It also **publishes** one property of its own, `--container-bleed`.

### The `--container-bleed` contract

`--container-bleed` says how far a child may pull out of the container to reach the edge of the screen.

The property inherits, so it reaches a child from a `.container` however far up the tree it sits. That makes the `var()` fallback each consumer writes its answer to a single question: what should happen when there is no `.container` above me at all?

There are exactly two answers, and which one you want depends on the direction the child pulls:

| The child... | Falls back to | Why |
| --- | --- | --- |
| Pulls out, via a negative margin or inset | `0px` | With nothing published, the child has no idea what its parent has to give. A gutter pulled out of a card, or out of a full-bleed block that is already full width, is a guess that overflows |
| Puts room back inside its own box | `var(--gutter)` | A full-bleed block spans the viewport with no inset of its own, so without this the content would touch the screen edge |

Inside a container the two collapse to the published value and the pair cancels out exactly.

```css
/* pulls out */
margin-inline: calc(var(--container-bleed, 0px) * -1);

/* puts room back in */
padding-inline: var(--container-bleed, var(--gutter));
```

### Keeping the token pair in sync

`--container-bleed-*` is a token per rung, and it is the sibling of `--container-max-width-*` in `@uncinq/component-tokens`. The two must change together:

- A rung whose max-width is `100%` bleeds by one `--gutter`. The container spans the viewport, so the child does reach the edge.
- A rung that caps bleeds by `0`. Dead space then sits on both sides, and a negative margin would stop short of the edge, reading as a misalignment rather than a bleed.

The two decisions live side by side in the same token file, so changing one without the other is a visible omission:

```css
--container-max-width-desktop: 100%;
--container-bleed-desktop: var(--gutter);
```

## `.grid`

A CSS grid wrapper whose column count follows the breakpoint.

```html
<div class="grid">
  <div>...</div>
  <div>...</div>
</div>
```

Children inherit `--grid-column` and apply it as their `grid-column`. To give one child a different span from its siblings, override the per-breakpoint property on that child:

```css
.featured {
  --grid-column-tablet: span 2;
  --grid-column-laptop: span 3;
}
```

The gap comes from `var(--grid-gap, var(--gap))`, so a local `--grid-gap` overrides the global gap without touching it.

## `.row`

A flex wrapper, stacked on mobile and turning into a row at the `--tablet` breakpoint and above.

Its column modifiers apply to direct children. Each one spans a share of the active column count, so the proportion holds whatever that count is, and lands on whole columns whenever the count is divisible, such as 12, 24 or 48.

| Modifier | Span | At 12 columns |
| --- | --- | --- |
| `.col-xsmall` | `cols / 3` | 1/3 |
| `.col-small` | `cols / 2` | 1/2 |
| `.col-medium` | `cols * 2 / 3` | 2/3 |
| `.col-large` | `cols * 5 / 6` | 5/6 |

The computed width is `span/cols * 100% - (1 - span/cols) * gap`. The second term gives back the gaps the item does not occupy, which is what keeps the proportions exact rather than approximate.

Two offset modifiers are available, from the `--tablet` breakpoint (768px) upwards, alongside the column modifiers:

| Modifier | Effect |
| --- | --- |
| `.offset-center` | `margin-inline: auto` |
| `.offset-end` | `margin-inline-start: auto` |

```html
<div class="row">
  <div class="col-small">...</div>
  <div class="col-small">...</div>
</div>
```

`.row` mirrors the grid's responsive column count into its own `--columns`, so `.col-*` resolves against the same tracks as `.grid`. A row and a grid placed side by side line up.

## Build requirement

All three files use `@media (--tablet)` and friends, which are `@custom-media` rules. Without [postcss-custom-media](https://www.npmjs.com/package/postcss-custom-media) in your build, those blocks are dropped and the layouts stay at their mobile values. See [Media queries](../mediaqueries/).
