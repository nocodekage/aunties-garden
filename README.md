# Her Garden

A four-chapter walk through my auntie's garden, rebuilt live in Three.js and lit
four different ways. Move the cursor to walk; scroll to go deeper.

## What it does

- Walks a live WebGL camera from the lawn, through the border, into the wood and
  out to a cherry tree in a glade. The cursor steers — it slides you across the
  path, tilts the view, and puts a footstep bob underneath you while you move.
- Renders the same garden under four treatments that crossfade into one another
  as you scroll, over one unbroken camera move:
  1. **Colour** — the garden as it stands, full afternoon.
  2. **Ink** — pen and a four-band grey wash on paper, edges found with a Sobel
     pass over a blurred copy so it draws masses rather than every leaf.
  3. **Moonlight** — a Fresnel rim gated on the moon vector, so every trunk,
     needle and leaf picks up a silver edge on the side turned to the moon.
  4. **Blossom** — sunset, falling petals, a real depth-of-field pass driven off
     the scene's depth buffer, focus locked to the cherry trunk. Two children
     sit underneath it reading.

## Where it comes from

Twenty-seven photographs of the garden, taken in about eleven seconds.

Every colour in the scene was sampled out of those frames rather than picked by
eye — the lawn's yellow-green (`#97bd5f`), the blue spruce silver (`#c4dacd`),
the gold wedding-cake dogwood (`#dad486`), the near-black conifers (`#111518`),
the dusk sky (`#f2edf0`). They are collected in the `P` table near the top of
the script.

The photographs make a poor *cutout* source — one fixed viewpoint of a garden
where everything overlaps, so there is no clean boundary to key against except
where foliage meets sky. So they are used for what they are actually good for:
the palette, and the far backdrop, where real tree-line silhouettes are keyed
against real sky. Everything nearer than the backdrop is built procedurally, and
that is also what makes the four treatments possible — rim lighting and
depth-of-field both need real geometry and real depth.

## How it is made

`index.html` holds the document, the CSS, the scene construction, the scroll and
cursor choreography, and the post-processing chain. A vendored Three.js r149
build supplies WebGL. No build step, no framework, no package manager.

Textures are painted at runtime onto 2D canvases and handed to Three as
`CanvasTexture` — bark, mown lawn, shredded mulch, leaf-mass sprites, hosta
clumps, blossom, petals. Geometry is generated: the woodland trunk field with a
glade carved around the cherry, the whorled blue spruce, the tiered dogwood, the
border, the fence, the statue.

```text
aunties-garden/
├── index.html                  the whole thing
├── her-garden.html             the same scene folded into one file
├── index-kage-original.html    the Kyoto temple this was built from
├── dist/                       what actually gets deployed
├── garden-assets/
│   ├── treeline-*.webp         backdrop silhouettes keyed from the photos
│   ├── maple-left/right.webp   the corner branches
│   └── kids-reading.webp
└── vendor/
    ├── fonts.css
    └── three.min.js            r149, MIT
```

## Run locally

```bash
python3 -m http.server 4173 --bind 127.0.0.1
```

Then visit [http://127.0.0.1:4173/](http://127.0.0.1:4173/). Any static server
works; there is no runtime network dependency.

## Debug switches

Appended to the URL:

| Param | Effect |
| --- | --- |
| `?shot=N` | jump straight to chapter N in a deterministic state |
| `?driver=timer` | drive the loop from `setTimeout` instead of `requestAnimationFrame` (a hidden tab never fires rAF) |
| `?grab=1` | keep the drawing buffer so a frame can be read back |
| `?q=low` | force the low-quality path |
| `?post=0` | bypass the whole post chain |
| `?dpr=N` | pixel-ratio cap |
| `?adapt=0` | freeze the resolution governor |
| `?nogl=1` | force the no-WebGL fallback |

## Credits

The garden is my auntie's. The two children come from character sheets of my own
and were re-rendered into summer clothes for the last chapter. The Three.js r149
runtime keeps its MIT licence notice.

Built on top of [Kage](https://github.com/MengTo/kage) by Meng To, which supplied
the single-file architecture — the fixed canvas under scrolling content, the
waypoint camera spline, the loading-job list, and the bloom pipeline.
