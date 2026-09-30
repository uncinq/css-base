---
isIndex: false
title: Abbreviations
description: A dotted underline on abbr[title], and why this is the one file in the layer with hardcoded values.
weight: 10
icon: fonts
---

`base/abbr.css` styles `abbr` only when it carries a `title`.

```css
abbr[title] {
  text-decoration-line: dotted;
  text-decoration-thickness: 1px;
  text-underline-offset: 0.3rem;
}
```

```html
<p>Published as <abbr title="Portable Document Format">PDF</abbr>.</p>
```

## Only with a title

An `<abbr>` with no `title` expands to nothing, so there is nothing to signal and the element is left unstyled. The attribute selector makes that explicit: the dotted underline is a promise that hovering or focusing reveals an expansion.

## The values are hardcoded

This is the one file in `@layer base` that does not read tokens. The dotted line is a convention rather than a design decision, and `1px` and `0.3rem` are tuned to it: a token-scaled thickness would turn the dots into dashes at large sizes, and the offset has to clear the descenders without drifting into the line below.

If you do want it token-driven, override the rule rather than waiting for a token:

```css
@layer base {
  abbr[title] {
    text-decoration-thickness: var(--text-decoration-thickness);
    text-underline-offset: var(--text-decoration-offset);
  }
}
```

## `title` is not enough on its own

A `title` is not reachable by keyboard and is inconsistently announced by screen readers. For an abbreviation that matters to comprehension, expand it in the text the first time it appears, and keep `abbr` for the recognition cue rather than for the definition itself.
