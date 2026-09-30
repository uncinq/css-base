---
isIndex: false
title: Code
description: Inline code, block code, kbd and samp, and the component-token-with-semantic-fallback pattern this file demonstrates.
weight: 8
icon: code-slash
---

`base/code.css` covers four elements and draws one distinction the user agent does not: inline `code` is styled as a chip, `code` inside `pre` is not.

```css
:where(code, kbd, samp) {
  font-family: var(--code-font-family, var(--font-family-code));
  font-size: var(--code-font-size, var(--font-size-xs));
}

:where(code):not(pre > code) {
  background-color: var(--code-color-background, var(--color-background-muted));
  border-radius: var(--code-border-radius, var(--radius-sm));
  color: var(--code-color-text, var(--color-text-on-muted));
  padding: var(--code-padding, var(--spacing-2xs));
}
```

| Selector | Styled as |
| --- | --- |
| `code`, `kbd`, `samp` | Monospace, one step down in size |
| `code` not inside `pre` | A tinted chip with padding and a radius |
| `pre` | A scrollable block with its own background and padding |
| `pre > code` | The chip styling removed again: no background, no padding |
| `kbd` | A chip with a real border, so a key reads as a key |

## Why `pre > code` is undone rather than avoided

`<pre><code>` is the correct markup for a code block, so the inner `code` matches the chip rule. Rather than writing a more convoluted selector, the file lets it match and then resets the three properties that do not belong:

```css
pre > code {
  font-size: var(--font-size-xs);
  background: none;
  padding: 0;
}
```

A chip inside a block would draw a second background over the first and pad every line.

## The token fallback pattern

This file is the clearest example of a convention used throughout the layer:

```css
font-family: var(--code-font-family, var(--font-family-code));
```

The component-scoped token comes first, the semantic one is the fallback. That gives two override levels from a single declaration:

| Set | Affects |
| --- | --- |
| `--code-font-family` | Code elements only |
| `--font-family-code` | Everything that reads the semantic monospace token |

Neither is declared by this package. Both are resolved from [@uncinq/design-tokens](../../../design-tokens/reference/) and [@uncinq/component-tokens](../../../component-tokens/reference/).

## Syntax highlighting

None is shipped. A highlighter adds its own classes inside `pre > code` and belongs in `@layer libs`, with any CSS you write to dress it in `@layer vendors`. See [Cascade layers](../../cascade-layers/#why-libs-and-vendors-are-a-pair).
