---
isIndex: false
title: Video
description: Fluid sizing on the native element, and where the aspect ratio and the controls come from instead.
weight: 14
icon: film
---

`base/video.css` makes the native element fluid and stops there.

```css
video {
  height: auto;
  max-width: 100%;
}
```

`max-width: 100%` keeps a video inside its container whatever its intrinsic width, and `height: auto` keeps the aspect ratio while it does so. The [reset](../../reset/) already sets `display: block` and `max-width: 100%` on `video` among the other media elements; this file adds the `height: auto` that makes the scaling proportional rather than squashed.

## What this file deliberately does not do

**It reserves no space.** A `<video>` sized only by `max-width` takes its height once the metadata loads, which shifts the rest of the page at an unpredictable moment. Reserving the box is the job of [`.video` in @uncinq/css-components](../../../css-components/embeds/video/), which declares an `aspect-ratio` from a token so the box exists at its final size from the first paint.

**It ships no controls.** Use the native `controls` attribute, or the [video toggle button](../../../css-components/buttons/btn-toggle-video/) when the design calls for a single play and pause control.

**It does not autoplay or mute anything.** Those are attributes on the element, and an autoplaying video that is not muted is blocked by every browser anyway.

## Accessibility

A `<video>` carrying meaning needs captions, through a `<track kind="captions">`, and a video that plays automatically needs a way to stop it. Neither is a styling concern, and neither is supplied here.

```html
<video controls poster="…">
  <source src="…" type="video/mp4">
  <track kind="captions" src="…" srclang="en" label="English" default>
</video>
```

A `poster` is worth setting even with a component that reserves the aspect ratio: it is what fills the box before the first frame decodes.
