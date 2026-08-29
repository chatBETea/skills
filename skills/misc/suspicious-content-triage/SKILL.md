---
name: suspicious-content-triage
description: Defensively triage a suspicious URL, email/message, or file for phishing, scam, or malware indicators through static analysis, without executing, opening, or submitting it anywhere live. Use when user shares a link, email, text message, or attachment/file and asks if it's safe, a scam, phishing, or malicious, or wants it checked before opening.
---

# Suspicious Content Triage

## Scope and boundaries

This skill produces a **defensive read on evidence**, not a guarantee. It never:

- executes, opens, or renders the content (no clicking links, no running scripts, no
  opening documents with macros enabled, no unpacking archives to executable locations)
- uploads the content to a third-party scanner (VirusTotal, urlscan.io, etc.) without
  the user's explicit go-ahead — hand hashes/URLs to the user to submit themselves
- helps craft phishing content, malware, or evasion techniques — if the request flips
  from "is this malicious" to "make this less detectable," stop and say no

If a verdict can't be reached from static evidence, report **Inconclusive** rather than
guessing — false confidence is worse than "I don't know."

## Process

1. **Classify the input**: a URL, message/email text, or a file. Ask for whichever
   wasn't provided if the type is ambiguous.
2. **For a URL or message** — see
   [url-and-message-indicators.md](url-and-message-indicators.md) for the full
   checklist (lookalike domains, redirect chains, urgency language, credential/payment
   requests, etc.).
3. **For a file** — see [file-static-analysis.md](file-static-analysis.md) for the
   static-only checklist (`file`, hashes, `strings`, script source review). Never
   execute the file to find out what it does.
4. **Report a verdict**: `Safe` / `Suspicious` / `Likely malicious` / `Inconclusive`,
   with the specific evidence that led there — not just the label.
5. **If suspicious or malicious**: tell the user not to click/open/reply, to report it
   to their security team or email provider, and — if they already interacted with it —
   to rotate any credentials entered and watch the account for the next few weeks.

## Reporting format

```
Verdict: <Safe|Suspicious|Likely malicious|Inconclusive>

Evidence:
- <finding> — <why it matters>
- ...

Recommended action: <what the user should do next>
```
