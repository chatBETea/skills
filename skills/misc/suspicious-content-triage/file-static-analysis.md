# File Static Analysis

Never execute, open, or double-click the file to see what it does. Every check here is
read-only.

## Baseline identification

- `file <path>` — compare the **actual** file type to the claimed extension (a `.pdf`
  that `file` reports as a PE executable or script is an immediate red flag).
- `sha256sum <path>` (and `md5sum` if the user wants to cross-reference an older
  report) — hand the hash to the user so *they* can look it up on VirusTotal/etc.
  themselves rather than you uploading the file.
- Note the file size and, for archives, list contents without extracting: `unzip -l`,
  `tar -tvf` — don't extract to a location where anything could auto-run.

## Script / macro-bearing files

- Read the source directly (`cat`, or `strings` for anything not plain text) — look
  for:
  - Obfuscation: base64/hex blobs immediately followed by decode+`eval`/`exec`/
    `Invoke-Expression`
  - Download-and-execute chains (`curl|bash`,
    `IEX (New-Object Net.WebClient).DownloadString(...)`, etc.)
  - Persistence attempts (registry run keys, cron/launchd entries, startup folder
    writes)
  - Anti-analysis tricks (sleep/delay loops, VM/sandbox detection, disabling AV)
- Office documents: check for macros (`olevba` from `oletools`, if available) rather
  than opening the file in Office — flag any macro that runs on `AutoOpen`/
  `Document_Open`.

## Binaries

- `strings <path> | less` — look for embedded URLs/IPs, suspicious command strings,
  hardcoded credentials, or debug/build paths that hint at origin.
- Note the format (`file`) and whether it's packed/stripped — heavy obfuscation on an
  unsigned binary is itself a signal, even without deeper reverse engineering.
- Do not run the binary, even in "just checking what it prints" mode — sandboxed
  dynamic analysis is out of scope for this skill.

## Verdict guidance

- Mismatched type/extension + obfuscated download-and-execute code →
  `Likely malicious`.
- Macro-bearing document with auto-run macros and no clear legitimate purpose →
  `Suspicious` at minimum.
- Clean strings, matching type, no auto-run behavior found → lean `Safe`, but say what
  wasn't checked (e.g. "no dynamic/sandbox analysis was performed").
