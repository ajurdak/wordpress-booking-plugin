# götterfunken — mark motion study

Five looping background directions for the three dots above the `ö` of the
götterfunken wordmark, plus the tech stack needed to ship one.

Open `index.html` in any browser. No build step, no dependencies, no network
access required — it is a single self-contained file.

## What's in it

| Direction | German | Loop | Idea |
| --- | --- | --- | --- |
| Constellation | Sternenzelt | 9.0 s | Dots become stars, link into a constellation, collapse back |
| Ember | Funkenflug | 8.0 s | Dots ignite, rise as embers trailing sparks, cool, fall home |
| Liquid | Schmelze | 7.0 s | Dots fuse into one metaball body, roam, pinch back into three |
| Orbit | Dreikörper | 10.0 s | Three-body orbit writing long-exposure trails, then captured |
| Signal | Raster | 9.0 s | Dots quantise into a halftone matrix that briefly resolves into a spark |

Each direction has a beat sheet that follows the playhead, and a transport
strip where every beat is clickable.

## How it is built

Canvas 2D, one `requestAnimationFrame` loop shared by all five stages.
Only stages intersecting the viewport are drawn. Device pixel ratio is capped
at 2. `prefers-reduced-motion` starts every stage paused on a poster frame.

Position is a **pure function of loop time** in all five directions — nothing
accumulates between frames. That is what makes the loops seamless and any
frame reproducible (and it is why the orbit trails can be drawn by
back-sampling the same function rather than by keeping a history buffer).

Production would move this to WebGL; see the stack table in the page.

## Swapping in the real logo

`computeLayout(ctx, w, h)` is the only place the mark is defined. It returns
the wordmark's position and the three home dot coordinates; every loop reads
`L.homes[i]` and `L.dotR` and nothing else. Replace its body with measurements
taken from the real logo SVG (`getBBox()` on the umlaut dots) and all five
directions re-target automatically.

The prototype currently sets the wordmark as `gotterfunken` — the umlaut is
omitted on purpose, because the animated dots *are* the umlaut.
