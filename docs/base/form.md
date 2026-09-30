---
isIndex: false
title: Inputs and labels
description: label, legend and every text-like input, the inset box-shadow outline, and the per-type handling for search, number, date and file.
weight: 17
icon: input-cursor-text
---

`base/form.css` is the largest file in the layer. It styles `label`, `legend` and every `input` that is not a button, a checkbox, a radio or a range. Largely inspired by [KNACSS](https://knacss.com/).

There is no `.input` or `.label` class. The selectors are the native elements.

## Labels and legends

```css
label, legend {
  color: var(--label-color-text);
  display: block;
  font-size: var(--label-font-size);
  font-weight: var(--label-font-weight);
  line-height: var(--label-line-height);
  padding-block: var(--label-padding-block);
  padding-inline: var(--label-padding-inline);
  text-align: var(--label-text-align);
  text-transform: var(--label-text-transform);
}
```

`display: block` puts the label on its own line, which is the layout a stacked form wants. The [checkbox](../form-checkbox/) and [radio](../form-radio/) files override the padding through `--label-padding` for their inline rows.

## Which inputs this file catches

```css
input:not(
  [type='button'],
  [type='reset'],
  [type='submit'],
  [type='checkbox'],
  [type='radio'],
  [type='range']
) { … }
```

Buttons are excluded because they are a component's job, not a base style, and the three excluded control types have their own files. Everything else, text, email, password, search, number, date, time, file and the rest, gets the same box.

| Token | Role |
| --- | --- |
| `--input-color-background` | Background |
| `--input-color-border` | Border colour, drawn as an inset shadow |
| `--input-color-border-hover` | Hover border colour |
| `--input-border-width` | Border thickness |
| `--input-border-radius` | Corner radius |
| `--input-color-text` | Text colour |
| `--input-color-placeholder` | Placeholder colour |
| `--input-padding` | Padding |
| `--form-transition` | Transition, under `prefers-reduced-motion: no-preference` |

## The border is an inset shadow

```css
border: 0;
box-shadow: inset 0 0 0 var(--input-border-width) var(--input-color-border);
```

This is the convention across every text-like control in the layer: [input](../form/), [textarea](../form-textarea/), [select](../form-select/), [checkbox](../form-checkbox/) and [radio](../form-radio/).

A real border occupies space, so thickening it on hover or focus moves the content by a pixel and nudges everything after it. An inset shadow is painted inside the box and takes no space at all, so a state change is purely a colour change and the layout never moves.

The [switch](../form-switch/) and the [range](../form-range/) thumb are the two exceptions. Both need a real `border`, because their outline is part of a shape that moves.

`::placeholder` sets `opacity: 100%` explicitly, because Firefox applies its own opacity to placeholder text and the token colour would otherwise be lightened twice.

## Per-type handling

| Type | Adjustment |
| --- | --- |
| `search` | `appearance: textfield`, and the four WebKit decoration and cancel pseudo-elements neutralised |
| `number` | `appearance: textfield`, and the spin buttons removed |
| `date`, `time`, `datetime-local` | `cursor: pointer`, and a calendar picker indicator that goes from 60% to full opacity on hover and focus |
| `file` | `cursor: pointer`, a tighter padding, and a styled `::file-selector-button` |

The file selector button reads `--form-color-accent` and `--form-color-text-on-accent`, with `--form-color-accent-hover` on hover, so it matches the accent used by the [checkbox](../form-checkbox/) tick without being a button component.

Most of these are wrapped in `:where()`, which contributes no specificity, so a component overrides them with a single class.

## States

```css
&:disabled       { opacity: var(--opacity-disabled); }
&:focus-visible  { outline: var(--focus-outline-width) var(--focus-outline-style) var(--focus-color-outline); }
```

Hover sits inside `@media (hover: hover) and (pointer: fine)` and excludes `:disabled`, so a disabled field never lights up and a touch device never leaves a field looking hovered.

## What is not here

**No layout.** This file styles controls, never their arrangement. The two-column grid, the help text and the check row belong to [`.form` in @uncinq/css-components](../../../css-components/forms/form/).

**No validation styling.** `:invalid` matches an empty required field before the user has typed anything, so styling it in a base layer marks a pristine form as wrong. Style `[aria-invalid='true']` in your own layer, set from your own validation code.

**No accessible names.** Every control needs a `<label for>` pointing at its `id`, or a [`.visually-hidden`](../accessibility/) label when the design has no room for a visible one.
