---
name: formative-check-builder
description: Build a short formative/diagnostic check (1-3 items) for one specific learning objective, where every wrong answer maps to a real, identifiable misunderstanding instead of being arbitrary — plus an instructor guide naming what each wrong answer reveals and what to do next. Use when asked to build a check for understanding, exit ticket, diagnostic quiz, or formative assessment for a lesson or topic.
---

# Formative Check Builder

This is a **diagnostic**, not a scored test — meant to show where a group of learners
currently stands in a few minutes, not to grade them. If the user wants a scored
exam with point values and a rubric, that's a different task; say so rather than
quietly relabeling this skill's output.

Adapted in spirit — not ported verbatim — from
[Anthropic's `k12-check-for-understanding`](https://github.com/anthropics/k12-teacher-skills)
(Apache-2.0). The original depends on a Learning Commons knowledge-graph connector
and K-12/CCSS-M-specific reference files that don't exist here; what's reused is the
mechanism (distractor-to-misconception mapping, a verification gate before shipping)
broadened to any subject and made self-contained.

## Process

1. **Clarify scope.** Get one specific learning objective, not a broad topic —
   "comparing fractions with unlike denominators," not "fractions." Ask for grade/
   level/background only if it isn't already given and would change item difficulty.
2. **Pick one assessable sub-skill and confirm it.** State the specific decision
   point the check will probe and why it's the right one to probe right now, then
   stop and wait for the user to confirm or redirect before building anything.
   Building the wrong check wastes more time than asking once.
3. **Design distractors around real misunderstandings.** For every wrong answer
   choice, name the specific, plausible misunderstanding a learner would need to
   hold to arrive at it — not an answer that's obviously wrong to anyone paying
   attention. Reason from general domain knowledge; if you're not confident a
   misconception is actually common, say that explicitly rather than presenting a
   guess as established research. Never invent a citation. When there are two or
   more wrong choices, make them different *kinds* of misunderstanding — e.g. one
   from a missing prerequisite skill, one from a current-topic conceptual slip —
   rather than two flavors of the same mistake; two near-duplicate distractors
   waste a question slot without adding diagnostic signal.
4. **Write two outputs, kept in sync:**
   - **Learner-facing**: the item prompt(s) and answer choices (or response space)
     only — no answer key, no source markers.
   - **Instructor-facing guide**: for every wrong choice, the misconception it
     reveals and one concrete next instructional step (a specific sub-skill or
     representation to re-teach — not "review the material").
5. **Verify before finishing.** For every item, check: (a) more than one choice is
   genuinely defensible-sounding, not a giveaway; (b) every distractor maps to a
   real reasoning path a learner could actually take, not noise; (c) no two wrong
   choices on the same item route to the same next step in the instructor guide —
   that's a sign two distractors are really the same misconception twice. Fix and
   recheck anything that fails these gates — skipping this step is the most common
   way this kind of check ends up useless.
6. **Close with options, not a blank "want changes?"** Offer 1-2 concrete
   alternatives: a different rigor level, a second item on the same sub-skill, or
   an open-response version instead of multiple choice.
