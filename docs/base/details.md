---
isIndex: false
title: Details
description: A bordered disclosure with a masked marker, an open state, and the summary + div contract it expects.
weight: 16
icon: caret-down-square
---

`base/details.css` is the largest non-form file in the layer. It styles the native disclosure element as a bordered card, replaces the user agent marker with a masked icon, and expects one structural convention.

```html
<details>
  <summary>How does the cascade work?</summary>
  <div>
    <p>The disclosed content.</p>
  </div>
</details>
```

The wrapping `<div>` is required. The file targets `summary + div` for the content padding, so content placed directly inside `<details>` is disclosed but unpadded.

## The container

| Token | Role |
| --- | --- |
| `--details-color-background` | Background |
| `--details-color-border` | Border colour when closed |
| `--details-color-border-open` | Border colour when `[open]` |
| `--details-border-radius` | Corner radius |
| `--details-border-style` | Falls back to `--border-style-normal` |
| `--details-border-width` | Falls back to `--border-width-sm` |
| `--details-box-shadow` | Resting shadow |
| `--details-box-shadow-hover` | Reassigned into `--details-box-shadow` on hover |
| `--details-color-text` | Text colour |
| `--details-margin` | Block margin |
| `--details-transition` | Falls back to `--transition-normal` |

Hover reassigns the token rather than redeclaring the property:

```css
&:hover {
  --details-box-shadow: var(--details-box-shadow-hover);
}
```

That is the pattern to copy when you add a state: change the variable, leave the declaration alone.

## The summary and its marker

`summary` is a flex row with the label at one end and the marker at the other, and it hides the native triangle twice, once with `list-style: none` and once with `::-webkit-details-marker`, because Safari needs both.

The marker is drawn on `::after` as a **mask over `currentColor`**, so it follows the summary's text colour rather than being a fixed-colour image:

```css
&::after {
  background-color: currentColor;
  mask: var(--details-icon) center / var(--details-icon-size) no-repeat;
}
```

`flex-shrink: 0` keeps it at full size when the label is long.

## The open state

```css
details[open] summary::after {
  mask-image: var(--details-icon-open, var(--details-icon));
  rotate: var(--details-icon-rotate-open, 180deg);
}
```

Two ways to signal open, and they compose. Leave `--details-icon-open` unset and the same glyph rotates 180 degrees, which suits a chevron. Set it to a different glyph and set `--details-icon-rotate-open: 0deg` to swap the icon instead, which suits a plus becoming a minus.

The rotation is transitioned under `--details-icon-transition`, which falls back to `rotate var(--duration-normal) var(--easing)`.

## Animating the disclosure itself

Not done here, and not free: `<details>` does not animate its own height. The [reset](../../reset/) enables `interpolate-size: allow-keywords` under `prefers-reduced-motion: no-preference`, which is the prerequisite for animating to `height: auto` in the browsers that support it. Add the transition in your own layer if you want it.

## Accessibility

The native element already exposes the expanded state, so **do not add `aria-expanded`**: it duplicates what the browser reports and can contradict it. A `<summary>` is a button in the accessibility tree and is keyboard-operable as shipped.
