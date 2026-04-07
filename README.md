# @uncinq/css-base

> Framework-agnostic CSS foundation — reset, native element styles, and layout primitives.

<img width="1280" height="640" alt="share-css-base" src="https://github.com/user-attachments/assets/3e57aa00-5349-4a2a-bdd5-3369b1ec21c2" />

## What's included

### Reset

Based on the [Josh W Comeau custom CSS reset](https://www.joshwcomeau.com/css/custom-css-reset/), with additions by Un Cinq:

- Box model normalization (`box-sizing: border-box`)
- Default margin removal
- `interpolate-size: allow-keywords` for keyword animations (behind `prefers-reduced-motion: no-preference`)
- Text rendering (`-webkit-font-smoothing`, `text-rendering`, `font-variant-ligatures`)
- Media defaults (`display: block`, `max-width: 100%` on `img`, `picture`, `video`, `canvas`, `svg`)
- Form controls font inheritance
- Overflow wrap on headings and paragraphs
- `text-wrap: pretty` on `p`, `text-wrap: balance` on headings

### Base

Styles for native HTML elements, each driven by design tokens from `@uncinq/design-tokens` and `@uncinq/component-tokens`:

| File | Element(s) |
| --- | --- |
| `base/accessibility.css` | `.visually-hidden`, `.visually-hidden-focusable` — a11y visibility helpers |
| `base/body.css` | `body` — background, text color, font family/size/weight/line-height |
| `base/headings.css` | `h1`–`h6` — color, font, size scale via `--font-size-heading-01…06` |
| `base/typography.css` | `p` — spacing, max-width |
| `base/link.css` | `a` — color, underline style, hover transition |
| `base/blockquote.css` | `blockquote`, `cite` — border-left, italic, muted color |
| `base/code.css` | `code`, `pre`, `kbd`, `samp` — monospace font, background, inline vs block |
| `base/list.css` | `ul`, `ol`, `li` — content list indent and spacing (prose context) |
| `base/address.css` | `address` — removes italic, resets paragraph spacing |
| `base/details.css` | `details`, `summary` — border, padding, open state, hover shadow |
| `base/figure.css` | `figure`, `figcaption` — flex layout, responsive caption direction |
| `base/form.css` | `label`, `legend`, `input`, `select`, `textarea`, `[type='checkbox']`, `[type='radio']` |
| `base/picture.css` | `picture` — `inline-block` display |
| `base/table.css` | `table`, `th`, `td`, `thead` — collapse, padding, header weight |
| `base/time.css` | `time` — small font size |
| `base/video.css` | `video` — fluid width, auto height |

### Layouts

Reusable layout utility classes:

| File | Class(es) |
| --- | --- |
| `layouts/container.css` | `.container` — centered, max-width responsive container with gutter |
| `layouts/grid.css` | `.grid` — CSS grid wrapper with responsive `--grid-column` custom property |
| `layouts/row.css` | `.row`, `.col-xsmall`, `.col-small`, `.col-medium`, `.col-large`, `.offset-center`, `.offset-end` |

---

## CSS cascade layers

All rules are scoped to named cascade layers, making every declaration safely overridable:

| Layer | Contents |
| --- | --- |
| `@layer config` | Reset and design tokens — foundational baseline, lowest priority |
| `@layer base` | Native element styles |
| `@layer layouts` | Layout utility classes |

The intended layer order for a full stack is:

```css
@layer config, base, layouts, vendors, components;
```

With cascade layers, the **last layer in the order wins**. Placing `config` first gives it the lowest priority — the reset and token defaults can never accidentally override base styles, component styles, or project overrides. `@layer base` and `@layer layouts` sit above it but below vendor and component styles.

Any layer can be overridden in your own project by importing after this package and writing to the same layer name:

```css
@import '@uncinq/css-base'; /* package defaults */

/* your project overrides — same layer, wins by order */
@layer base {
  a {
    --color-link: #0070f3;
  }
}
```

→ MDN: [Using CSS cascade layers](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)

---

## Installation

```bash
npm install @uncinq/css-base
# or
yarn add @uncinq/css-base
```

---

## Usage

```css
/* everything — reset + base + layouts */
@import '@uncinq/css-base';

/* or by group */
@import '@uncinq/css-base/css/reset.css';
@import '@uncinq/css-base/css/base.css';
@import '@uncinq/css-base/css/layouts.css';

/* or file by file */
@import '@uncinq/css-base/css/base/body.css';
@import '@uncinq/css-base/css/base/headings.css';
@import '@uncinq/css-base/css/layouts/container.css';
@import '@uncinq/css-base/css/layouts/row.css';
```

---

## File structure

```
css/
  index.css                   ← imports reset, base, layouts in order
  reset.css                   ← Josh W Comeau reset + Un Cinq additions, in @layer config
  base.css                    ← barrel, imports all base/*
  layouts.css                 ← barrel, imports all layouts/*
  base/
    accessibility.css         ← .visually-hidden, .visually-hidden-focusable
    blockquote.css            ← blockquote + cite — border-left, italic, muted color
    code.css                  ← code, pre, kbd, samp — monospace, background, padding
    list.css                  ← ul, ol, li — content list indent and spacing
    address.css               ← address — removes italic, resets paragraph spacing
    body.css                  ← body — background, color, font, rendering
    details.css               ← details + summary — border, padding, open/hover states
    figure.css                ← figure + figcaption — flex layout, responsive captions
    form.css                  ← label, input, select, textarea, checkbox, radio
    headings.css              ← h1–h6 — color, font family, size scale
    link.css                  ← a — color, underline, hover transition
    picture.css               ← picture — inline-block display
    table.css                 ← table, th, td — collapse, padding, header weight
    time.css                  ← time — xs font size
    typography.css            ← p — spacing, max-width
    video.css                 ← video — fluid width
  layouts/
    container.css             ← .container — centered, responsive max-width
    grid.css                  ← .grid — CSS grid with responsive column custom property
    row.css                   ← .row + column/offset modifiers
```

---

## Peer dependencies

This package requires [`@uncinq/design-tokens`](https://github.com/uncinq/design-tokens) and [`@uncinq/component-tokens`](https://github.com/uncinq/component-tokens) to resolve the CSS custom properties it references. It does not bundle any token values itself.

Token categories referenced across base and layout files include:

- `--color-bg`, `--color-text`, `--color-heading`, `--color-link`, `--color-border`
- `--font-family-text`, `--font-family-heading`
- `--font-size-*`, `--font-weight-*`, `--line-height-*`
- `--spacing-*`, `--max-width-*`
- `--transition-normal`
- `--border-width-*`, `--border-style-*`, `--radius-*`
- Component-scoped tokens: `--input-*`, `--select-*`, `--textarea-*`, `--checkbox-*`, `--radio-*`, `--details-*`, `--table-*`, `--figure-*`, `--label-*`
- Layout tokens: `--gutter`, `--gap`, `--columns`, `--container-max-width-*`, `--flex-*`, `--spacing-section`

Install both peer dependencies:

```bash
npm install @uncinq/design-tokens @uncinq/component-tokens
```

→ [`@uncinq/design-tokens`](https://github.com/uncinq/design-tokens) — primitive and semantic design tokens (color, typography, spacing, …)
→ [`@uncinq/component-tokens`](https://github.com/uncinq/component-tokens) — component-scoped tokens (form, table, figure, …)

Then import them before this package so the tokens resolve correctly:

```css
@import '@uncinq/design-tokens';
@import '@uncinq/component-tokens';
@import '@uncinq/css-base';
```

---

## References

- [`@uncinq/design-tokens`](https://github.com/uncinq/design-tokens) — primitive and semantic design tokens
- [`@uncinq/component-tokens`](https://github.com/uncinq/component-tokens) — component-scoped tokens
- [Josh W Comeau custom CSS reset](https://www.joshwcomeau.com/css/custom-css-reset/) — the reset foundation used in `css/reset.css`
- [MDN: CSS cascade layers](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)
