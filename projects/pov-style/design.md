# Design — "Point of View" Motion Style

Editorial, data-driven motion graphics. Minimalist, dark, atmospheric deep-blue
backgrounds. Smooth, slow, confident transitions. White type on near-black navy.

## Mood

- Calm, cinematic, technical. Swiss/editorial layout with mono micro-labels.
- The frame feels like an instrument panel over an atmospheric blue scene.
- Motion is smooth and eased — nothing snaps. Data reveals, line draws, slow drifts.

## Palette

| Token         | Hex / value             | Use                                                                                                                                                                |
| ------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `--bg-0`      | `#070C16`               | Deepest navy (vignette center, base)                                                                                                                               |
| `--bg-1`      | `#0B1424`               | Base background navy                                                                                                                                               |
| `--steel`     | `#3E5C8A`               | Mid atmospheric blue (portal glow)                                                                                                                                 |
| `--fog`       | `#5B7396`               | Foggy steel-blue (haze, mid distance)                                                                                                                              |
| `--horizon`   | `#A8BFD8`               | Cool light — horizon line, sky glow                                                                                                                                |
| `--accent`    | `#6E9BD8`               | Cool secondary accent (nodes, faint data)                                                                                                                          |
| `--highlight` | `#FFD60A`               | **Intense yellow** — THE highlight color, complementary to navy. All animated highlights, route lines, key data values, active emphasis. One strong hit per scene. |
| `--ink`       | `#F4F7FB`               | Primary text (near-white)                                                                                                                                          |
| `--muted`     | `#8492A8`               | Mono micro-labels, captions                                                                                                                                        |
| `--line`      | `rgba(255,255,255,.22)` | Hairline strokes, card borders, geometry                                                                                                                           |

Monochrome-first (white on navy). **`--highlight` (intense yellow `#FFD60A`) is the one
hero accent** — complementary to the blue, used for animated highlights, route/dashed
paths, and key data values. One strong yellow hit per scene; never flood the frame with it.

## Backgrounds (look = the 3 reference photos)

Three atmospheric deep-blue moods, all dark, all smooth:

1. **Portal / tunnel** — radial glow: lighter steel-blue around the edges fading
   to a dark center. Slow scale/drift.
2. **Fog** — flat desaturated steel-blue haze with faint silhouettes, soft drifting
   blobs. Low contrast.
3. **Sea horizon** — vertical gradient, one bright thin horizon line mid-frame with
   a soft reflection below. Calm.

**Banding rule (critical):** blue gradients on dark H.264-band badly. Always use
**radial** gradients (not full-screen linear), keep transitions soft, and overlay a
low-opacity **SVG fractal-noise grain** (`feTurbulence`) to dither. Never a hard
full-screen linear gradient.

## Typography

- **Display:** `Inter`, weight 800. Tight tracking (`-0.02em`). Big — 110px+ for hero.
- **Micro-labels / data:** `IBM Plex Mono`, 12–14px, uppercase, `letter-spacing: .14em`,
  color `--muted`. Corner chrome, timecodes, units, captions.
- **Data values:** `Inter` 700 with `font-variant-numeric: tabular-nums` (counters
  must not jitter).
- Min sizes for video: 60px+ headlines, 20px+ body, 13px+ labels.

## Frame chrome (editorial)

Persistent mono micro-labels in the 4 corners + an index number, like a contact
sheet. Hairline registration marks optional. Constant, quiet, `--muted`.

## Motion

- **Eases:** `power3.out` / `expo.out` for entrances; `sine.inOut` for drifts;
  `power2.inOut` for crossfades. Vary at least 3 eases per scene.
- **Line/geometry reveals:** animate `stroke-dashoffset` (draw-on). Core signature.
- **Data reveals:** count-up values (tabular-nums), bars/lines growing via scaleX
  from a fixed origin, smooth.
- **Transitions:** crossfades and soft wipes only. No jump cuts. ~0.6–0.9s, overlapping.
- **Map overlays ("Karten-Einblendungen") — primary data element:** a dark map
  (stylized, desaturated, on-palette) with: a **glowing marker dot** (pulsing rings,
  finite repeats), an **animated dashed route line** (amber `--route`) that draws on
  via `stroke-dashoffset` then flows with marching-ant dashes (intense yellow `--highlight`),
  faint **place labels**
  (`IBM Plex Mono`, `--muted`), a hairline lat/long **graticule**, and a **count-up
  data callout** (distance/coords, tabular-nums). Everything draws/eases on smoothly —
  no hard pop-in. This is the signature "Einblendung". See reference.
- Offset first tween 0.1–0.3s. No `repeat:-1` (finite repeats only). Deterministic
  (no `Math.random`/`Date.now`).

## Don'ts

- No pure-black `#000` fills, no `#3b82f6`/`Roboto` defaults, no full-screen linear
  gradients, no hard cuts, no snappy/bouncy eases, no more than one accent per scene.
