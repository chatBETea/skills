# Post-Production and Delivery

## Edit workflow order

Rough assembly (every usable take, roughly in order) → fine cut (trimmed to the
actual story, pacing locked) → picture lock (no more structural changes) → sound
design/mix and color (often run in parallel once picture is locked — they're
independent tracks) → final export. Treat picture lock as a real gate: sound and
color work started before it gets redone every time the edit changes again
afterward, which is expensive and avoidable.

## Cut types

- **Hard cut** — the default, one shot straight to the next.
- **J-cut** — the next scene's audio starts under the current shot before its
  picture does.
- **L-cut** — the current scene's audio continues after its picture has cut away.
- **Match cut** — visual/compositional similarity bridges two otherwise unrelated
  shots.
- **Jump cut** — same subject, same framing, a visible time skip — uses the
  convention openly rather than hiding it.
- **Cutaway** — breaks to a secondary subject or action, often to cover a
  continuity gap.

## Color

Correction before grading, always. Correction matches exposure and white balance
across shots from the same scene so they look like they belong together; grading is
the creative pass (a look, a mood) applied on top of footage that's already
matched. Grading before correction compounds every exposure/white-balance mismatch
instead of fixing it. Use scopes (waveform, vectorscope, histogram) to judge
exposure and color objectively — a monitor alone isn't reliable enough to grade on,
especially an uncalibrated one.

## Delivery

Every platform and broadcast outlet publishes its own current spec for resolution,
aspect ratio, codec, max bitrate, max duration, and caption requirements — these
change often enough that a memorized number is a real risk. Check the platform's
current creator/help documentation at export time rather than reusing a past
project's settings or a number from memory.

What's stable regardless of platform: keep an archival master at the highest
quality the project was finished in, separate from every compressed delivery
export. Re-deriving a lower-quality export from a master is always possible later;
the reverse is not.
