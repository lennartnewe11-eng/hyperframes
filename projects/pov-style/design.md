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

- **Hero keywords (serif):** `Playfair Display`, weight 700–800. High-contrast Didone —
  the headline word of each beat. Big (120px+ landscape), centered, often sliding
  horizontally through frame. This is the primary "spoken word" treatment.
- **Display / secondary (sans):** `Inter`, weight 800. Tight tracking (`-0.02em`).
  Used inside band-wipe panels and for non-headline words.
- **Micro-labels / data:** `IBM Plex Mono`, 12–14px, uppercase, `letter-spacing: .14em`,
  color `--muted`. Corner chrome, timecodes, units, captions.
- **Data values:** `Inter` 700 with `font-variant-numeric: tabular-nums` (counters
  must not jitter).
- Min sizes for video: 60px+ headlines, 20px+ body, 13px+ labels.

## Frame chrome (editorial)

Persistent mono micro-labels in the 4 corners + an index number, like a contact
sheet. Hairline registration marks optional. Constant, quiet, `--muted`.

## Word Visualization — keyword-morph system (PRIMARY)

The video's job is to **visualize what is said** — words and facts — synced to the
transcript. Based on the reference clip:

- **One keyword per beat.** Pull the key word/phrase from each spoken line, set it big
  and centered (serif hero), surrounded by negative space. Secondary words live in
  band-wipe panels (sans).
- **Words flow into each other ("ineinander übergehen")** — never a plain fade. Use:
  - **Horizontal slide-through:** the serif word travels across center, bleeding off both
    edges, as the next word arrives. `x` translate, `power3.inOut`.
  - **Particle / dot constellation:** a field of small dots scatters then reconverges
    around the next word (deterministic positions, seeded). The accent dots are
    **`--highlight` yellow** (the reference used blue — we use yellow).
  - **Band-wipe / invert:** a solid horizontal band slides vertically across frame,
    inverting fg/bg (white band, black text) and carrying the next keyword. Hard edge,
    no opacity crossfade. This is the signature transition — matches "minimal blending".
- **Fact motifs** tied to specific words: radial **burst lines**, **audio-equalizer bars**,
  circle-in-rectangle, count-up numbers, map overlays. One motif per fact, drawn on.
- **Persistent thin baseline rule** (hairline) anchors the lower third across beats.
- **Background:** near-black deep navy (`--bg-0`) for keyword beats, like the reference.
  The atmospheric blue moods (portal / fog / horizon) are reserved for occasional
  full-beat emphasis, not every scene.

## Motion

- **Eases:** `power3.out` / `expo.out` for entrances; `sine.inOut` for drifts;
  `power2.inOut` for crossfades. Vary at least 3 eases per scene.
- **Line/geometry reveals:** animate `stroke-dashoffset` (draw-on). Core signature.
- **Data reveals:** count-up values (tabular-nums), bars/lines growing via scaleX
  from a fixed origin, smooth.
- **Transitions:** hard graphic only — **band-wipes / inverts, particle morphs, horizontal
  slides**. As little opacity-blending as possible. Crossfades are a rare exception, not the
  default. No jump cuts either; the graphic move IS the transition. ~0.5–0.8s.
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

- No `#3b82f6`/`Roboto` defaults, no full-screen linear gradients, no snappy/bouncy eases,
  no more than one accent per scene.
- **No opacity crossfades as the default transition** — favour band-wipes, particle morphs,
  and slides (minimal blending).
- Don't crowd beats: one keyword + at most one fact motif per beat. Negative space is the look.
