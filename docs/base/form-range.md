---
isIndex: false
title: Range
description: The slider, its two vendor track and thumb implementations, and the .range-value output that follows the thumb via anchor positioning.
weight: 20
icon: sliders
---

`base/form-range.css` styles `[type='range']` plus two optional classes, `.range` and `.range-value`.

```html
<div class="range" data-range-output="anchor">
  <label for="volume">Volume</label>
  <input type="range" id="volume" min="0" max="100" value="50">
  <output class="range-value" for="volume">50</output>
</div>
```

## Two vendor implementations, not one

A range input has no common styling API. WebKit and Gecko expose different pseudo-elements, so the track and the thumb are declared twice:

| Pseudo-element | Engine | Styles |
| --- | --- | --- |
| `::-webkit-slider-runnable-track` | WebKit, Blink | Track |
| `::-webkit-slider-thumb` | WebKit, Blink | Thumb |
| `::-moz-range-track` | Gecko | Track |
| `::-moz-range-thumb` | Gecko | Thumb |
| `::-moz-range-progress` | Gecko | The filled part before the thumb |

They cannot be combined into one selector list: an unrecognised pseudo-element invalidates the whole rule, so each engine needs its own block.

`::-moz-range-progress` has no WebKit equivalent, so the filled portion of the track is shown in Firefox and not in Chrome or Safari. That is a known asymmetry rather than an oversight.

| Token | Role |
| --- | --- |
| `--range-track-color` | Track colour (WebKit) |
| `--range-color-track` | Track colour (Gecko) |
| `--range-track-height` | Track thickness, also its border radius |
| `--range-thumb-size` | Thumb diameter |
| `--range-thumb-color` | Set on the input as `color`, so both thumbs read it through `currentcolor` |
| `--range-thumb-color-border` | Thumb border colour (WebKit) |
| `--range-thumb-border-width` | Thumb border thickness (WebKit) |

The thumb colour is routed through the input's own `color` property, which is why both vendor thumbs can use `currentcolor` and stay in sync from a single token.

The WebKit thumb is centred on the track with a negative top margin:

```css
margin-block-start: calc((var(--range-track-height) - var(--range-thumb-size)) * 0.5);
```

Change `--range-track-height` or `--range-thumb-size` and the centring follows, with nothing else to adjust.

## The thumb keeps a real border

This is one of the two controls in the layer that does not use the [inset box-shadow convention](../form/#the-border-is-an-inset-shadow). The other is [the switch](../form-switch/). A thumb is a shape that travels along a track, so its outline is part of the shape rather than a state on a static box.

## The value output

`.range` is a positioning context with room reserved below it, and `.range-value` is an absolutely positioned chip inside it. By default the chip sits at the start of the track and is translated down.

With `data-range-output="anchor"` on the wrapper, and in a browser that supports anchor positioning, the chip **follows the thumb**:

```css
@supports (position-anchor: initial) {
  [data-range-output="anchor"] ::-webkit-slider-thumb { anchor-name: --thumb; }
  [data-range-output="anchor"] ::-moz-range-thumb      { anchor-name: --thumb; }

  [data-range-output="anchor"] .range-value {
    position-anchor: --thumb;
    position-area: bottom;
    translate: 0 0;
  }
}
```

The whole block is inside `@supports`, so a browser without anchor positioning keeps the static chip rather than losing it. Both are readable; only one tracks the thumb.

Updating the text inside the `<output>` is your code's job. The CSS positions the chip and nothing else:

```js
input.addEventListener('input', () => { output.value = input.value; });
```

## Accessibility

The native element is already a slider to assistive technology, operable with the arrow keys, and it reports its own `min`, `max` and value. Do not add `role="slider"` or `aria-valuenow`.

Use `<output for="…">` rather than a `<span>` for the value, so the number is announced as the result of the control rather than as loose text. Focus uses the shared `--focus-*` ring, and `:disabled` drops to `--opacity-disabled`.
