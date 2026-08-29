---
name: web-design-review
description: Review UI code against current web interface design guidelines — accessibility, interaction, layout, typography — fetched fresh from the source on every run instead of relying on static advice baked into this file. Use when asked to review UI, check accessibility, audit a design, review UX, or check a site against web design best practices.
---

# Web Design Review

Fetches Vercel's continuously-updated Web Interface Guidelines and checks the given
files against them, rather than relying on a snapshot that goes stale the moment a
new pattern (or a deprecated one) ships.

Adapted from [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)
(MIT license), encountered by way of seeing it used inside
[Multica](https://github.com/multica-ai/multica)'s own skill-import pipeline. Author:
Vercel.

## Process

1. Fetch the current guidelines before every review — don't rely on a cached memory
   of them:

   ```
   https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md
   ```

2. If the user gave a file or pattern, read those files. If not, ask which files to
   review.
3. Check every file against every rule in the fetched guidelines.
4. Report findings in the terse `file:line` format the fetched guidelines specify —
   don't invent a different report format.

## Why fetch instead of hard-code

Interface guidelines move fast. A hard-coded rule list drifts out of date without
anyone noticing; fetching fresh keeps this skill's advice current with zero
maintenance here.
