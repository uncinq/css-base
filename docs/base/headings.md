---
isIndex: false
title: Headings
description: One shared rule for h1 to h6, and a per-level size token that each element reassigns.
weight: 3
icon: type-h1
---

`base/headings.css` styles `h1` to `h6` with a single rule, then gives each level its size by reassigning one custom property.

```css
h1, h2, h3, h4, h5, h6 {
  color: var(--color-heading);
  font-family: var(--font-family-heading);
  font-size: var(--font-size-heading);
  font-weight: var(--font-weight-heading);
  letter-spacing: var(--letter-spacing-heading);
  line-height: var(--line-height-heading);
  margin-block: var(--spacing-heading);
  max-width: var(--max-width-heading);
  text-transform: var(--text-transform-heading);
}

h1 { --font-size-heading: var(--font-size-heading-01); }
h2 { --font-size-heading: var(--font-size-heading-02); }
/* … through h6 */
```

## Why the indirection

`font-size` is declared once, against `--font-size-heading`, and each element supplies the value. That keeps the shared rule to a single block, and it gives you two override levels:

```css
/* every h2 on the site */
h2 { --font-size-heading: 2rem; }

/* one heading, whatever its level */
.hero h1 { --font-size-heading: 4rem; }
```

The six `--font-size-heading-01` to `-06` tokens reference the T-shirt scale (`--font-size-xl`, `-lg`, `-md`, `-sm`) in [@uncinq/design-tokens](../../../design-tokens/reference/). Remapping a level is a token change, not a rule rewrite.

## The rest of the shared properties

| Token | Role |
| --- | --- |
| `--color-heading` | Heading colour, distinct from `--color-text` |
| `--font-family-heading` | Display typeface, often different from the body one |
| `--font-weight-heading` | Shared weight |
| `--letter-spacing-heading` | Usually negative on large display sizes |
| `--line-height-heading` | Tighter than the body line-height |
| `--spacing-heading` | Block margin above and below |
| `--max-width-heading` | Measure cap, so a long title wraps before it runs the page width |
| `--text-transform-heading` | `none` by default |

## Wrapping

`h1` to `h6` get `text-wrap: balance` and `overflow-wrap: break-word` from the [reset](../../reset/), not from this file. Balanced wrapping is what keeps a two-line title from leaving one orphaned word on the second line.
