# @uncinq/css-base

> Framework-agnostic CSS foundation — reset, native element styles, and layout primitives.

<img width="1280" height="640" alt="share-css-base" src="https://github.com/user-attachments/assets/3e57aa00-5349-4a2a-bdd5-3369b1ec21c2" />

## Installation

```bash
npm install @uncinq/css-base @uncinq/design-tokens @uncinq/component-tokens
```

Both token packages are peer dependencies: this package carries no values of its own and resolves every custom property it references from them.

## Usage

Declare the cascade layer order once at the top of your entry stylesheet, **before any import**, then import in this order:

```css
@layer reset, tokens, libs, base, vendors, layouts, components, pages, utilities;

@import '@uncinq/design-tokens';    /* @layer tokens */
@import '@uncinq/css-base';         /* @layer reset, base, layouts */
@import '@uncinq/component-tokens'; /* @layer tokens */
@import '@uncinq/css-components';   /* @layer components */
```

Groups and individual files are importable too:

```css
@import '@uncinq/css-base/css/base.css';
@import '@uncinq/css-base/css/base/headings.css';
```

Your build must run [postcss-custom-media](https://www.npmjs.com/package/postcss-custom-media), otherwise the breakpoints in `css/mediaqueries.css` are dropped silently.

## What's included

| Group | Layer | Scope |
| --- | --- | --- |
| Reset | `@layer reset` | Baseline normalization, based on the [Josh W Comeau reset](https://www.joshwcomeau.com/css/custom-css-reset/) |
| Base | `@layer base` | 23 files styling native HTML elements from tokens |
| Layouts | `@layer layouts` | `.container`, `.grid`, `.row` and their modifiers |
| Media queries | none | The `--sm` / `--md` / `--lg` / `--xl` custom media scale |

## Documentation

Full documentation: **[socle.uncinq.dev/docs/css-base/](https://socle.uncinq.dev/docs/css-base/)**

It is also versioned with the code in [`docs/`](docs/), one page per element and per layout primitive, and ships inside the npm package, so it is readable offline and from `node_modules`:

- [Overview](docs/overview.md) — installation, import order, file structure
- [Cascade layers](docs/cascade-layers.md) — the three layers, and why the order is yours to declare
- [Reset](docs/reset.md)
- [Base](docs/base/) — 23 pages, one per file
  - Text — [body](docs/base/body.md), [headings](docs/base/headings.md), [paragraphs](docs/base/paragraphs.md), [links](docs/base/link.md), [lists](docs/base/list.md), [blockquotes](docs/base/blockquote.md), [code](docs/base/code.md), [address](docs/base/address.md), [abbreviations](docs/base/abbr.md), [superscript](docs/base/sup.md)
  - Media — [figures](docs/base/figure.md), [picture](docs/base/picture.md), [video](docs/base/video.md), [tables](docs/base/table.md), [details](docs/base/details.md)
  - Forms — [inputs and labels](docs/base/form.md), [checkboxes](docs/base/form-checkbox.md), [radios](docs/base/form-radio.md), [range](docs/base/form-range.md), [select](docs/base/form-select.md), [switch](docs/base/form-switch.md), [textarea](docs/base/form-textarea.md)
  - [Accessibility helpers](docs/base/accessibility.md)
- [Layouts](docs/layouts/) — [`.container`](docs/layouts/container.md) and the `--container-bleed` contract, [`.grid`](docs/layouts/grid.md), [`.row`](docs/layouts/row.md)
- [Media queries](docs/mediaqueries.md)

## References

- [`@uncinq/design-tokens`](https://github.com/uncinq/design-tokens) — primitive and semantic design tokens
- [`@uncinq/component-tokens`](https://github.com/uncinq/component-tokens) — component-scoped tokens
- [`@uncinq/css-components`](https://github.com/uncinq/css-components) — the component layer built on this package
- [MDN: CSS cascade layers](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)

## License

MIT © [Un Cinq](https://uncinq.dev/)
