---
name: video-production
description: Reference for film/TV and video production craft — shot types and camera movement, three-point lighting, audio recording levels, the standard post-production workflow, color correction/grading order, and platform delivery specs. Use when planning a shoot, reviewing footage or a cut, choosing shots or lighting setups, or preparing an export for a specific platform or broadcast.
---

# Video Production

Covers film/TV and video craft end to end: pre-production and production (planning
the shoot) in [pre-production-and-production.md](pre-production-and-production.md),
post-production through delivery (editing, color, export) in
[post-production-and-delivery.md](post-production-and-delivery.md).

Adapted in spirit — not ported verbatim — from the `video-production` skill in
[travisjneuman/.claude](https://github.com/travisjneuman/.claude) (MIT license).

## Process

1. Identify which phase the task is actually in — planning a shoot, reviewing or
   giving feedback on footage, editing, color, or exporting — and read only that
   reference file. Don't load both for a narrow question.
2. For anything with a numeric platform spec (resolution, bitrate, max duration,
   codec), don't rely on a memorized number. Platform specs change; point the user
   to that platform's current creator/help documentation to confirm the live
   number rather than stating one as fact. The craft principles here (shot
   grammar, lighting ratios, edit order, color-before-grade) are stable; exact
   platform numbers are not.
3. Never run actual rendering, encoding, or AV synthesis as part of using this
   skill — this is planning/review guidance, not a production pipeline.

## Quality checklist (any phase)

- Does every shot or cut serve the story, or is it there by habit?
- Is audio intelligible before anything else is judged — a technically perfect
  image with bad audio fails first.
- Is color correction (matching exposure/white balance across shots) done and
  signed off *before* any creative grading pass starts, not interleaved with it?
- For delivery: has the *current* spec for the target platform or broadcast been
  checked today, not assumed from memory or a past project?
