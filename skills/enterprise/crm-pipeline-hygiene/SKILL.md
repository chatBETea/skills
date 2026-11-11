---
name: crm-pipeline-hygiene
description: Audit a CRM export (contacts, leads, or deals as CSV/spreadsheet) for the data-quality issues that quietly erode B2B/B2C pipeline reliability — duplicates, stale deals, missing required fields, inconsistent stage naming, orphaned owners. Use when asked to clean up, audit, or sanity-check a CRM export, pipeline, or contact/deal list from Salesforce, HubSpot, Pipedrive, or similar.
---

# CRM Pipeline Hygiene

Produces a findings report, not automatic changes. This skill never merges, deletes,
or writes back to a live CRM on its own — every fix it recommends is something the
user (or their CRM's own dedup/merge tool) applies deliberately, because a wrong
auto-merge silently destroys history that's expensive to reconstruct.

## Process

1. **Get the export.** Ask for the file (CSV/XLSX) if not provided, and which CRM it
   came from — field names differ (HubSpot's `Deal Stage` vs Salesforce's
   `StageName` vs Pipedrive's `stage_id`), and getting this wrong produces false
   positives. For an export from something else, or a generic/already-normalized
   CSV, use the column headers as given rather than guessing a mapping to one of
   the three above.
2. **Run the checks** (skip any that don't apply to the given object type):
   - **Duplicates** — same email, phone, or company domain across multiple contact
     rows. Report matched groups, not a silent auto-merge.
   - **Stale/orphaned deals** — no activity within a reasonable window for that
     pipeline, no owner assigned, or sitting in a stage far past its typical
     duration relative to the rest of the dataset.
   - **Missing required fields** — no email on a contact, no value/stage/close-date
     on a deal, no owner on either.
   - **Inconsistent categorical values** — stage/status strings that are the same
     concept spelled differently (`Closed Won`, `closed-won`, `Won`) — these
     fragment reporting even when the data is otherwise complete.
   - **Owner concentration** — one owner holding a disproportionate share of
     open deals or contacts (bus-factor risk, not necessarily a data error, but
     worth surfacing).
3. **Report by category**, each with a count and a handful of representative
   example rows — not a full dump of every offending row, which buries the signal.
4. **Recommend, don't execute.** For duplicates specifically, name which record
   looks like the canonical one (most complete, most recently active) and which
   look like the merge candidates, but leave the merge itself to the user.

## Reporting format

```
## <Category> — <count> found

- <example row summary>
- <example row summary>
- ...

Recommended action: <what to do, and why it's not done automatically>
```
