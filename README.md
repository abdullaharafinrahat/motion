# motion-story-video v4 — "Director's Cut"

Upgraded skill that adds a **Creative Director** stage (the missing piece — v1/v2 generated the
same film every time because the example storyboard was the only taste in the system).

## Contents
| File | What it is |
|---|---|
| `motion-story-video-v4.skill` | The full upgraded skill (install like the original `.skill`) |
| `sample_directors_cut_v4.mp4` | 18s sample film rendered with the v4 grammar + seed-25 creative brief |
| `motion-story-video-v4-directors-cut.zip` | Everything above + the binding `creative_brief.json` + README |
| `creative_brief_sample_seed25.json` | The brief that directed the sample (topic: "why templates kill motion design") |

## What v4 changes
1. **Creative Director** (`scripts/creative_director.py` + `references/creative-director.md`)
   - Deterministic, topic-unique `creative_brief.json` with 9 BINDING axes: world metaphor,
     camera personality, palette mutation, texture kit, type attitude, pacing, signature motif,
     signature transition, lighting plan. `--seed N` for another take.
   - **Novelty gate**: `--check` ERRORS on storyboards reusing the example's cut skeleton,
     titles, accent, or 27.0s shape (the v1/v2 demos shared cuts 3.67/7.47/11.8/23.2s frame-for-frame).
2. **Engine v4**: 900px wide-angle lens, opt-in depth compensation, velocity-aware directional
   motion blur (1x cost), spotlight pool, elevation shadows, glass HUD, halftone/scanline/torn-paper
   shaders, new `zsandwich` / `clipping` / `inkloop` / `glasshud` components.
3. **Craft gates** (`scripts/lint_v4_craft.py`): no hero text in `space:"hud"`, micro-event ≤ 1.15s
   gap, ≥3 depth planes/scene, real camera move on scenes ≥3s, template-reuse check.

Built from `main` (commit e843a8d) + v2 patch (df77cf1) forensics. Sample is 720p30 with layered
foley audio.
