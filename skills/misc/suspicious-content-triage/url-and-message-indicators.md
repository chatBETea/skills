# URL and Message Indicators

## Domain and link checks

- Compare the **displayed** text/domain to the actual href/target — mismatches are a
  strong signal.
- Look for lookalike/typosquatted domains (`paypa1.com`, `arnazon.com`, extra hyphens,
  wrong TLD).
- Decode punycode/IDN homograph domains (`xn--...`) before judging them.
- Flag raw IP-address URLs (`http://185.23.44.1/login`) — legitimate services almost
  never link this way.
- Flag URL shorteners (`bit.ly`, `tinyurl`, etc.) hiding the real destination — resolve
  one redirect hop at a time with `curl -sI --max-redirs 0 <url>` rather than following
  blindly, and stop if it 302s somewhere unexpected.
- Note the TLD — free/high-abuse TLDs (`.tk`, `.xyz`, `.top`, etc.) paired with a brand
  name in the path/subdomain is a common phishing pattern.
- If WHOIS/registration-age tooling is available, a domain registered in the last few
  days is a red flag; if it isn't available, say so rather than guessing.

## Message/email content checks

- **Urgency/fear pressure**: "your account will be suspended," "act within 24 hours,"
  "unauthorized login detected."
- **Requests for credentials, OTPs, gift cards, wire transfers, or crypto** —
  legitimate services don't ask for these over email/SMS.
- **Generic greeting** ("Dear Customer") on a message claiming to be from a specific
  known relationship.
- **Sender/reply-to mismatch** — display name says one org, the actual address is
  unrelated or a free-mail domain.
- **Spoofed authority**: impersonating IT, a bank, a delivery service, a government
  agency, or someone's boss (BEC-style).
- **Attachment or link as the "next step"** immediately after a pressure statement.

## Verdict guidance

- 2+ strong signals (lookalike domain, credential harvesting page, urgency + payment
  ask) → `Likely malicious`.
- 1 signal with a plausible legitimate explanation → `Suspicious`, ask the user for
  more context before finalizing.
- No signals found, but you can't verify the sender/domain independently →
  `Inconclusive` — don't call it `Safe` just because nothing looked wrong.
