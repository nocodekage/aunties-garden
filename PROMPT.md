# Build prompt

Create a single-page WebGL experience called **Her Garden**: a four-chapter walk
through a real suburban back garden, rebuilt live and lit four different ways.
It should feel like walking, not like watching a slideshow.

## The garden

- Work from photographs of the real place. Sample the palette out of the frames
  rather than choosing colours by eye, and say so in the code.
- Build the planting procedurally: a woodland trunk field with uneven spacing and
  a mix of heavy and slender trees, a canopy of leaf-mass sprites, a whorled blue
  spruce, a tiered gold dogwood, burgundy maples, a mixed border of shrubs and
  hostas with an undulating front edge, a mown lawn, a mulch bed, a black metal
  fence, a white statue.
- Carve a glade in the wood and stand a cherry tree in it.
- Use the photographs only where they are genuinely better than geometry: the far
  backdrop, keyed where foliage meets sky.

## The four lights

One garden, four treatments, crossfading over a single unbroken camera move:

1. **Colour** — full afternoon, warm key, saturated greens.
2. **Ink** — pen and a four-band grey wash on paper. Find edges with a Sobel pass
   over a *blurred* copy so it draws the boundaries between masses instead of
   scribbling over every leaf. Keep the wash off pure white or bright ground
   flattens into a blank band.
3. **Moonlight** — a Fresnel rim gated on the moon direction and attenuated with
   distance, so it reads as backlight instead of a uniform glow. Ground planes
   opt out; a grazing-angle rim lights a lawn like a runway.
4. **Blossom** — sunset, falling petals, depth of field driven off a real depth
   attachment with focus locked to the cherry trunk. Two children sit under the
   tree reading. Blur toward a blurred copy of the whole scene, never toward the
   bloom chain.

## Movement

- Scroll drives depth along a camera spline; interpolate position and look-at on
  separate curves.
- The cursor is the walk: it slides the camera across the path, tilts the view,
  and runs a footstep bob and a little roll while it is moving. The bob settles
  when the cursor stops.
- Step the camera back along its own view axis on tall frames so the composition
  does not crop.

## Quality

- One HTML file. No framework, no build step, no runtime network dependency.
- Keep the loop alive: wrap the frame body so one bad frame cannot silently
  freeze the page, and log it once.
- Ship debug switches — a deterministic per-chapter state, a timer-driven loop, a
  readable-frame flag, and quality overrides.
- Degrade to a readable article when WebGL is unavailable.
- Verify at desktop and at 390×844, check every asset for 404s, watch the
  console, and confirm no horizontal overflow before shipping.
