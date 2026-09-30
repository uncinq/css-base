---
isIndex: false
title: Switch
description: A track and knob built from a checkbox with role="switch", moved with flex-grow rather than translation.
weight: 22
icon: toggle-on
---

`base/form-switch.css` styles `[role="switch"]`. Largely inspired by [KNACSS](https://knacss.com/).

```html
<input type="checkbox" role="switch" id="notify">
<label for="notify">Email notifications</label>
```

The markup is a checkbox carrying the role. That is what makes the state work for free: the browser tracks `:checked` and assistive technology announces on and off rather than checked and unchecked, with no JavaScript involved.

[`base/form-checkbox.css`](../form-checkbox/) excludes it explicitly, with `[type='checkbox']:not([role='switch'])`, so the two files never both apply.

## The track and the knob

| Token | Role |
| --- | --- |
| `--switch-width` | Track width |
| `--checkable-size` | Track height and knob diameter, falls back to `1.25rem` |
| `--switch-track-color` | Track colour, unchecked |
| `--switch-track-color-checked` | Track colour, checked |
| `--switch-thumb-color` | Knob colour, unchecked |
| `--switch-thumb-color-checked` | Knob colour, checked |
| `--switch-thumb-scale` | Knob size inside the track |
| `--switch-color-border` | Track border colour |
| `--switch-border-width` | Track border thickness |
| `--switch-border-radius` | Corner radius, shared by track and knob |

The knob is `::after`, and `::before` is an empty spacer.

## The knob moves with flex-grow

```css
&::before { content: ""; }

&:checked::before { flex-grow: 1; }
```

The track is an `inline-flex` with `justify-content: start`. The spacer starts at zero width, so the knob sits at the start. When the switch is checked, the spacer grows to fill the free space and pushes the knob to the end.

Nothing computes a travel distance. Change `--switch-width` or `--checkable-size` and the knob still lands exactly at the end, because the spacer is whatever is left over. A `translateX` implementation would need that distance recalculated every time either token changed.

The transition is declared on the host, on `::before` and on `::after` separately, all inside `@media (prefers-reduced-motion: no-preference)`.

## It keeps a real border

This is one of the two controls in the layer that does not use the [inset box-shadow convention](../form/#the-border-is-an-inset-shadow). The other is the [range](../form-range/) thumb. The track's outline is part of a shape the knob travels inside, so it has to occupy space.

## Switch or checkbox

A switch takes effect **immediately**: flipping it turns something on. A checkbox records a choice that a submit button later applies. Choosing the wrong one is an accessibility problem rather than a styling one, since the announced role tells the user which to expect.

Note that this file declares no `+ label` rule of its own, unlike the [checkbox](../form-checkbox/) and the [radio](../form-radio/). Lay the switch out beside its label with [`.form-check` in @uncinq/css-components](../../../css-components/forms/form-check/).

## Accessibility

`role="switch"` is all that is needed. Do **not** add `aria-checked`: on a real checkbox the browser already reports the state, and a hand-managed attribute drifts out of sync with `:checked` the moment the user clicks.
