# Matt Pocock's Skills

A collection of agent skills (Claude Code slash commands / behaviors) distributed as a
plugin (`mattpocock-skills`). There is no build, no runtime, no tests — the deliverable
is markdown. "Correctness" here means: skills are well-formed, discoverable, and every
index that references them (`README.md`, bucket `README.md`s, `.claude-plugin/plugin.json`)
is in sync with what's actually on disk.

## Repo layout

```
CLAUDE.md              this file
CONTEXT.md             domain glossary (issue tracker, issue, triage role)
README.md              top-level skill index + project pitch (public-facing)
LICENSE
.claude-plugin/
  plugin.json           list of skills shipped by the plugin (paths into skills/)
.out-of-scope/          written decisions *not* to build something (see below)
docs/
  adr/                  architecture decision records for this repo itself
scripts/
  link-skills.sh        symlinks every non-deprecated skill into ~/.claude/skills
  list-skills.sh         lists every SKILL.md in the repo
skills/
  engineering/          daily code work — indexed, in the plugin
  productivity/         daily non-code workflow tools — indexed, in the plugin
  misc/                 kept around but rarely used — indexed, in the plugin
  personal/             tied to the maintainer's own setup — NOT indexed, NOT in the plugin
  in-progress/          drafts not yet ready to ship — NOT indexed, NOT in the plugin
  deprecated/           no longer used — NOT indexed, NOT in the plugin, excluded from link-skills.sh
```

Each bucket folder (`skills/<bucket>/`) has its own `README.md` listing every skill in
that bucket with a one-line description.

## The indexing rule (enforced)

This is the load-bearing convention of the repo — keep these in sync whenever a skill
is added, renamed, removed, or moved between buckets:

- Every skill in `engineering/`, `productivity/`, or `misc/` **must** have:
  - an entry in `.claude-plugin/plugin.json` (`./skills/<bucket>/<name>`)
  - a linked entry in the top-level `README.md` under its bucket's "Reference" section
  - a linked entry in its bucket's own `README.md`
- Skills in `personal/`, `in-progress/`, and `deprecated/` **must not** appear in
  `plugin.json` or the top-level `README.md`. They still get an entry in their own
  bucket `README.md`.
- Every "linked" reference above must link the skill name to its `SKILL.md`
  (e.g. `**[tdd](./skills/engineering/tdd/SKILL.md)**`).
- Moving a skill between buckets means updating all three places (or removing entries,
  if moving into a non-indexed bucket).

When adding, removing, or moving a skill, treat these as one atomic change — don't
leave `plugin.json` or a README out of sync with `skills/`.

## Anatomy of a skill

```
skills/<bucket>/<skill-name>/
├── SKILL.md            required — frontmatter + instructions
├── <topic>.md           optional — split-out reference material, linked from SKILL.md
└── scripts/             optional — deterministic helper scripts
```

`SKILL.md` frontmatter:

```yaml
---
name: skill-name # matches the directory name
description: What it does. Use when [specific triggers].
disable-model-invocation: true # optional; only for skills that must be run explicitly
---
```

- `description` is the only thing the agent sees when deciding whether to load the
  skill — first sentence is what it does, second is "Use when...". Keep it concrete
  and trigger-rich (see `skills/productivity/write-a-skill/SKILL.md` for the full
  writeup on this).
- Split content out of `SKILL.md` into sibling `.md` files once it exceeds ~100 lines,
  or once a section is a distinct sub-domain (e.g. `tdd/mocking.md`, `tdd/tests.md`).
  Keep references one level deep.
- Use `disable-model-invocation: true` for skills that scaffold per-repo config or
  otherwise should only run when explicitly invoked (e.g.
  `setup-matt-pocock-skills`, `zoom-out`).
- To create a new skill, use the `write-a-skill` skill
  (`skills/productivity/write-a-skill/SKILL.md`) rather than freehanding the structure.

## Per-repo dependency: `setup-matt-pocock-skills`

Several engineering skills (`to-issues`, `to-prd`, `triage`, `diagnose`, `tdd`,
`improve-codebase-architecture`, `zoom-out`) assume per-*consuming*-repo config seeded
by `setup-matt-pocock-skills`: which issue tracker to use, the triage label vocabulary,
and where domain docs (`CONTEXT.md` / `docs/adr/`) live. Per
`docs/adr/0001-explicit-setup-pointer-only-for-hard-dependencies.md`:

- **Hard-dependency** skills (`to-issues`, `to-prd`, `triage`) explicitly tell the user
  to run `/setup-matt-pocock-skills` if the config wasn't provided — without it their
  output is wrong, not just less sharp.
- **Soft-dependency** skills (`diagnose`, `tdd`, `improve-codebase-architecture`,
  `zoom-out`) reference the domain glossary/ADRs in passing prose and degrade
  gracefully if they're absent.

Preserve this split when editing those skills — don't add a hard setup dependency to a
soft-dependency skill or vice versa without updating the ADR.

## This repo's own domain glossary

`CONTEXT.md` at the root defines the ubiquitous language for *this* repo (not for repos
that consume the plugin): **Issue tracker**, **Issue**, **Triage role**. Keep it current
if terminology here shifts — this is the same convention the `grill-with-docs` skill
teaches consuming repos to maintain.

## `.out-of-scope/`

Each file here is a standing decision *not* to build something, with the reasoning and
prior request references. Before implementing a feature request that smells like one of
these (new issue-tracker backend, a question cap on grilling sessions, a verify/check
mode for `setup-matt-pocock-skills`), check this directory first — and if the user
still wants it, surface the existing rationale before proceeding rather than silently
overriding it.

## Scripts

- `scripts/list-skills.sh` — enumerates every `SKILL.md` in the repo (includes
  non-indexed buckets). Use this to sanity-check the indexing rule above.
- `scripts/link-skills.sh` — symlinks every skill except `deprecated/` into
  `~/.claude/skills` for local testing. Refuses to run if `~/.claude/skills` is itself a
  symlink back into this repo (would self-pollute the working tree).

## Workflow for common changes

- **Add a skill**: pick the right bucket, scaffold via `write-a-skill`, then add an
  entry to the bucket's own `README.md`, and — if the bucket is indexed (`engineering/`,
  `productivity/`, `misc/`) — also add entries to `plugin.json` and the top-level
  `README.md`, all in the same change.
- **Deprecate a skill**: move the directory to `skills/deprecated/`, remove its entry
  from the *source* bucket's `README.md` (and, if the source bucket was indexed, from
  `plugin.json` and the top-level `README.md` too), then add it to
  `skills/deprecated/README.md`.
- **Edit an existing skill**: just edit `SKILL.md` / its reference files; no index
  changes needed unless the name or bucket changes.
- **Reject a feature request**: if it matches an existing `.out-of-scope/*.md` decision,
  point to it; otherwise, if the user and you agree it's out of scope, add a new file
  there rather than only stating it in conversation.

There is no lint/build/test step to run after changes — verification is manual: check
that the three index locations agree with the contents of `skills/`, and that any new
or edited `SKILL.md` follows the frontmatter and structure conventions above.
