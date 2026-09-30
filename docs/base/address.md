---
isIndex: false
title: Address
description: Undoing the user agent italic on address, and the paragraph spacing that would otherwise break it apart.
weight: 9
icon: geo-alt
---

`base/address.css` exists to undo two things rather than to add any.

```css
address {
  font-style: normal;
  margin-block: 0;

  p {
    margin-block: 0;
    text-wrap: unset;
  }
}
```

| Rule | Undoes |
| --- | --- |
| `font-style: normal` | The user agent italic, which reads as emphasis rather than as contact details |
| `margin-block: 0` | The block margin, left to the surrounding layout |
| `p { margin-block: 0 }` | The [paragraph](../paragraphs/) rhythm, which would space the lines of a single address apart |
| `p { text-wrap: unset }` | `text-wrap: pretty` from the [reset](../../reset/) |

## Why `text-wrap` is reset

The reset applies `text-wrap: pretty` to every `p`, which tunes the wrapping of running prose by avoiding a short last line. An address is a stack of short deliberate lines, usually broken with `<br>`, so there is nothing for the algorithm to improve and it can only move a break the author chose.

`unset` rather than `nowrap`: the address still wraps if it has to, it simply stops being treated as prose.

## Markup

```html
<address>
  Un Cinq<br>
  12 rue de la Paix<br>
  75002 Paris
</address>
```

`<address>` is for the contact details of the nearest `<article>` or of the document, not for arbitrary postal addresses in body text. Using it as a generic address wrapper gives a screen reader a contact landmark that is not one.
