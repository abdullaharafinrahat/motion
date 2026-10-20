# motion-story-video — v2 (patched)

Patched version of the `motion-story-video.skill`. A shot-by-shot comparison against a
reference-quality motion-graphics edit surfaced several gaps **beyond** "the camera never
moves" — this branch closes them and ships a new sample rendered with the patched engine.

## Files

| File | What it is |
|---|---|
| `motion-story-video.skill` | Patched skill package (drop-in replacement for the v1 archive, same format) |
| `demo_deploy_day_v2.mp4` | **New sample** rendered with the patched engine — 27 s, 1280×720, 30 fps, 7 scenes, collage-world explainer |
| `demo_editorial_explainer.mp4` | Original v1 sample, kept for comparison |

## Gaps found → what was patched

### 1. The camera never moves (the known gap)
- `SKILL.md` + `references/motion-fundamentals.md`: a **camera plan is now mandatory** — one line per scene (`scene — move — reason`) written *before* any scene JSON. Every scene ≥ 3 s must get a real move: push-in landing at zoom 1.08–1.18, pan ≥ 60 px, dolly-through ≥ 150 px, or a jump-cut punch-in with `ease:"hold"`. A locked-off shot is only valid as a deliberate choice with handheld drift.
- `scripts/lint_storyboard.py`: a scene ≥ 3 s on a locked-off camera is now a **lint ERROR**; moves too small to read (zoom < .08, pan < 40 px) raise a WARN. Self-test codified: stills at scene start vs midpoint must differ visibly.

### 2. No depth — every element sat on one plane (no parallax)
- `assets/engine/core.js`: the camera track now supports `x, y, zoom, dolly, rot, rx, ry` with true perspective — elements at different `z` parallax naturally as the camera moves; footage backgrounds take a `parallax` factor.

### 3. No collage world — graphics felt pasted onto footage instead of being the world
- `assets/engine/components.js` + `backgrounds.js` + `style.css`: new components —
  - `cardwall`: a tilted wall of cards in real perspective that the camera **dollies through** (cards at different z = parallax planes)
  - `cluster`: a stat moment where the number rolls while photo chips burst around it
  - `speaker`: the matted talking head standing **inside** the graphics (footage mode), with idle bob and depth
  - `tag` / `bubble` with `leader` elbow connectors that pin labels to the exact thing they name

### 4. No continuity technique — every scene restarted from scratch
- `references/motion-fundamentals.md`: documents the pro **match-cut trick** — carry a scene's end camera state (same zoom/x/y) into the next scene's start key across a hard cut, plus a persistent `overlay` object, so the world keeps moving while the content changes.

### 5. The bundled example demonstrated none of this
- `assets/example/storyboard.example.json` (~150 lines changed) and `storyboard.vertical.example.json`: rebuilt around the collage world with real camera keys in every scene, so the shipped example *is* the standard.

### 6. The linter let slop through
- `scripts/lint_storyboard.py` v3 rules: static-camera ERROR, invisible-move WARN, camera `zoom < 1` on footage (reveals frame edges) WARN, HUD/zoom crop checks, existing hard-cut / transition-streak checks kept.

## Engine version
Engine bumped to **v3** (deterministic 3D motion engine: camera, easing, shape morph,
cardwall / cluster / speaker collage world, leader lines, 3D tilt, transitions, bundled fonts).
Anti-slop rules unchanged: no neon, no purple gradients, no particles, no glow/glass.
