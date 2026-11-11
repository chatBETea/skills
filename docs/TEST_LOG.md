# Test Log

Manual test results for skills in this repo. There is no automated test suite (see
`CLAUDE.md`'s "There is no lint/build/test step" note) — this log is the record of
the manual verification that stands in for one: structural indexing checks, and
functional dry-runs against constructed test cases for individual skills.

Entries are chronological, most recent first. Each functional entry records the
method used, what was found, and the commit that fixed it (if any) — a clean pass is
recorded too, so "untested" and "tested, found nothing" stay distinguishable.

## Standing known issue

**`misc/` bucket skills missing from `plugin.json`** — `git-guardrails-claude-code`,
`migrate-to-shoehorn`, `scaffold-exercises`, and `setup-pre-commit` are indexed in
`skills/misc/README.md` and the top-level `README.md`, but were never added to
`.claude-plugin/plugin.json`. Pre-existing, not introduced by any entry below. Flagged
repeatedly during structural passes; not yet fixed pending a decision on whether to
fix it in place.

## 2026-11-11

### Structural indexing pass (full repo)

**Method:** `scripts/list-skills.sh` plus a diff of every bucket's `README.md`,
`.claude-plugin/plugin.json`, and the top-level `README.md`'s Reference section
against the actual `skills/*/*/SKILL.md` directories on disk. Also a YAML-frontmatter
sanity pass over every `SKILL.md` (parses, `name` matches its directory, `description`
present).

**Result:** PASS, repeated 4 times across the session (after each new skill added).
Clean every time except the standing known issue above.

### `illustration-critique` — light-source consistency — FAIL → FIXED

**Method:** Dry-run against a constructed example (an elf-archer illustration with a
longer bow arm, a quiver shadow falling the opposite direction from the rest of the
figure's light source, flat value range, a dead-center composition competing with a
glowing moon, and a color palette stated as an intentional choice).

**Finding:** Four of five checks caught their seeded issue cleanly, including
correctly *not* flagging the intentional color choice. The light-source mismatch only
surfaced because the tester already knew illustration-critique conventions and forced
it under "value structure" — the checklist text itself never named light-source
direction as its own check, so following it literally could miss that specific,
common defect.

**Fix:** `3e3ea6c` — named light-source-direction consistency explicitly inside the
value-structure check.

### `illustration-critique` — pre-ship dry-run — PASS

**Method:** Dry-run against a different constructed example (a production-ready
warrior character: shortened sword arm, a perspective mismatch between figure and
background, flat value range, unclear face/sword focal hierarchy, a clashing
unintentional color combination) before the skill was ever indexed.

**Result:** All five checks caught their seeded issue, in the documented order, with
the right specificity. No fix needed at this pass — the gap above wasn't found until
a second, independent test case later the same day.

### `video-production` — routing + fact-check — PASS / FIXED

**Method 1 (fact-check, pre-ship):** Reviewed every concrete technical claim in the
skill against known industry convention before indexing.

**Finding:** The edit-workflow-order section asserted sound mix must happen strictly
before color; in practice the two typically run in parallel once picture is locked.

**Fix:** Corrected in `3857340` (same commit that added the skill — caught before
ship, not after).

**Method 2 (routing, post-ship):** Routed three concrete requests ("color temperature
for an interview near a window," "footage from two cameras looks mismatched,"
"export settings for a TikTok ad") through the skill's own "pick the right reference
file" instruction.

**Result:** All three routed to the correct reference file and answered from the
correct section, including the delivery-spec question correctly refusing to state a
memorized platform number.

### `suspicious-content-triage` — phishing + malicious-file fixtures — PASS

**Method:** Built two fixtures — a phishing email (typosquat domain
`paypa1-secure-verify.com`, raw IP-address URL, urgency language, generic greeting,
credential request, spoofed sender) and a malicious file (`readme.txt` that is
actually a bash script hiding a base64-encoded `curl | bash` chain) — and walked both
through the skill's documented checklists (`url-and-message-indicators.md`,
`file-static-analysis.md`) by hand.

**Result:** Every seeded signal was caught exactly as written; verdict guidance
correctly landed both at "Likely malicious." No gap found, no fix needed.

### `web-design-review` — live dependency check — PASS

**Method:** Confirmed the skill's fetch URL
(`raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md`) is
live (HTTP 200) and that its content actually matches what the skill claims (a rule
list plus a `file:line` output format). Repeated at multiple points in the session;
consistently live.

### `formative-check-builder` — dry-run, pre-ship — FAIL → FIXED

**Method:** Dry-run against a constructed example (building a diagnostic check for
"adding fractions with unlike denominators," 5th grade) before the skill was indexed.

**Finding:** The adaptation (from Anthropic's `k12-check-for-understanding`) had
dropped two structural rules from the original that don't depend on the
knowledge-graph/reference-file infrastructure the adaptation deliberately excluded:
distractors should represent different *kinds* of misunderstanding, not two flavors
of the same mistake; and no two wrong answers should route to the same instructor
next-step.

**Fix:** Restored in `47ea2b5` (same commit that added the skill — caught before
ship).

### `crm-pipeline-hygiene` — fixture dry-run — FAIL → FIXED

**Method:** Built two CSV fixtures (contacts with a duplicate email and a missing
required field; deals with a stale 2025-dated deal among otherwise Nov-2026 deals,
inconsistent stage naming `Closed Won` vs `closed-won`, a missing owner, and one
owner holding 6 of 7 deals) and walked the skill's documented process against them.

**Finding:** All five check categories (duplicates, stale/orphaned deals, missing
required fields, inconsistent categorical values, owner concentration) caught their
seeded issue. Gap: step 1 assumed the export came from Salesforce, HubSpot, or
Pipedrive by name, with no fallback for a generic/already-normalized export — exactly
what the test fixtures were.

**Fix:** `129da03` — added an explicit fallback for exports outside the three named
CRMs.
