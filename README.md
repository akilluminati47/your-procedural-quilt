# Procedural Quilt

An infinite procedural zoom — in the spirit of zoom-quilt art, but every image
layer and every sound is **generated at runtime in the browser**. No image
files, no audio files, no third-party assets: one self-contained `index.html`.

## How it works

- **10 scenes** are painted onto 2048×2048 canvases from a seeded RNG:
  gradient skies, mandala rays, mountains, tentacles, floating eyes, mushroom
  spires, checkerboard plains, orbs, clouds.
- Each scene ends in a **portal** (eye, mouth, stone arch, glowing rings,
  vortex, or gear) with a transparent hole punched in the centre.
- The player nests the layers at 5× scale steps and zooms continuously —
  each portal opens onto the next world, looping forever.
- **Audio** is a generative WebAudio patch: a slow detuned drone with a
  breathing lowpass filter, plus echoing pentatonic bells. It starts
  **unmuted** on the entry click (the click-to-enter gesture satisfies
  browser autoplay policy, so no muted fallback is needed).

## Performance

Each scene is baked once into an offscreen canvas with the same routine that
draws it live, so a cached blit and a live repaint are pixel-identical
wherever they meet — no seam. Only the one or two layers magnified past the
cache's native size repaint as live vectors each frame; everything farther out
is a single cheap `drawImage`. A frame-time governor eases render resolution
down when frames run long and back up when there's headroom, seeded from an
initial guess based on the device's own pixel budget — so one page holds its
framerate from a phone up to a 4K display.

## Controls

| Input               | Action                        |
|---------------------|-------------------------------|
| click / tap         | enter (starts sound)          |
| scroll / ↑ ↓        | zoom speed (can reverse)      |
| two-finger drag ↕   | zoom speed (touch)            |
| space               | pause                         |
| M                   | mute / unmute                 |
| R                   | generate a new world          |
| C                   | seed cycle (new world / 47s)  |
| F                   | fullscreen                    |
| mouse move / finger | gentle parallax drift         |

The world drifts to follow the cursor, but settles back to neutral over the
top-right controls and whenever the cursor fades from idleness. The custom
cursor itself fades out after 3s of inactivity and fades back in on movement.

Fully touch-driven on phones and tablets: one finger steers the parallax and
the start-screen lens, two fingers pinch-drag up/down for zoom speed. The
start screen scales down for narrow portrait screens.

## Seed cycle & the zipper

Press **C** (or the ▶ control) to start the seed cycle: every 47 seconds the
world hops to a fresh random seed. Each hop plays a **zipper transition** —
the world unzips down the middle along a wavy, hue-cycling tear with
interlocking teeth and a glowing pull, peels into black where the new seed
swaps in, then zips closed over the next world. The tear's direction and tempo
follow the zoom you were riding (downward when zooming forward, upward in
reverse, faster at higher speed). Toggling the cycle off returns to the seed
you started from.

## Seeds

The world is deterministic per seed. The seed lives in the URL hash —
`index.html#239015077` always regenerates the same ten worlds, so you can
bookmark or share a favourite.
