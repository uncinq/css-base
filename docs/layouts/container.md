---
isIndex: false
title: Container
description: The centred wrapper, its responsive max-width, and the --container-bleed contract every full-bleed child depends on.
weight: 2
icon: bounding-box
---

A centred wrapper with a responsive max-width and a gutter on both sides.

```html
<div class="container">...</div>
```

```css
.container {
  --container-bleed: var(--container-bleed-mobile);
  margin-inline: auto;
  max-width: var(--container-max-width, var(--container-max-width-mobile));
  padding-inline: var(--gutter);
  width: 100%;
}
```

| Property read | Role |
| --- | --- |
| `--container-max-width-*` | Max-width per breakpoint rung |
| `--container-bleed-*` | Bleed allowance per rung, the paired token |
| `--gutter` | Inline padding |

Each breakpoint reassigns both `--container-max-width` and `--container-bleed` together, at `--tablet`, `--tablet-wide`, `--laptop` and `--desktop`.

It also **publishes** one property of its own, `--container-bleed`.

## The `--container-bleed` contract

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

[`.scrollsnap` in @uncinq/css-components](../../../css-components/utilities/scrollsnap/) is the clearest consumer of the first form: it pulls a row out to the screen edge and relies entirely on the published value to know how far.

## Keeping the token pair in sync

`--container-bleed-*` is a token per rung, and it is the sibling of `--container-max-width-*` in [@uncinq/component-tokens](../../../component-tokens/reference/). The two must change together:

- A rung whose max-width is `100%` bleeds by one `--gutter`. The container spans the viewport, so the child does reach the edge.
- A rung that caps bleeds by `0`. Dead space then sits on both sides, and a negative margin would stop short of the edge, reading as a misalignment rather than a bleed.

The two decisions live side by side in the same token file, so changing one without the other is a visible omission:

```css
--container-max-width-desktop: 100%;
--container-bleed-desktop: var(--gutter);
```

## Nesting

Nesting one container inside another applies the gutter twice and republishes `--container-bleed` at the inner value, which is almost never what you want. Use a single container per region and let [`.grid`](../grid/) or [`.row`](../row/) divide the space inside it.
