---
isIndex: false
title: Picture
description: One declaration, and the descender gap it is there to make predictable.
weight: 13
icon: images
---

`base/picture.css` is the shortest file in the package.

```css
picture {
  display: inline-block;
}
```

## Why it exists

`<picture>` has no user agent styling of its own. It is an inline wrapper whose only job is to hold `<source>` elements around an `<img>`, and by default it is `display: inline`, which means it cannot take a width, a height or a vertical margin.

`inline-block` makes it a box: it sizes to its `<img>`, it flows with the text, and it accepts dimensions when a component needs to give it some.

The [reset](../../reset/) already makes `img` itself `display: block` with `max-width: 100%`. Without this file, that block-level image would sit inside an inline parent, which is legal but leaves the wrapper's own line box in the layout.

## Inside a figure it becomes a block

[`base/figure.css`](../figure/) sets `figure picture { display: block }`. In a figure the picture is a layout element rather than a piece of inline content, and `inline-block` would leave the descender gap under the image, a few pixels of whitespace that look like a spacing bug and are not one.

That is the pattern to follow in your own components: keep `inline-block` where the image flows with text, set `block` where it does not.

```css
.card picture {
  display: block;
}
```
