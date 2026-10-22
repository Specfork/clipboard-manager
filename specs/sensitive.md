# Sensitive data

**Status:** Draft · **Prefix:** SEN

## Purpose

Clipboard contents include passwords, tokens and private text. These rules decide what is never stored, how stored items can be hidden, and what the app never reveals.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| SEN-1 | Concealed content is never stored, shown, notified, synchronised or logged: on Linux the password-manager hint format set to `secret`; on macOS the concealed pasteboard type; on Windows any of the formats that exclude content from clipboard history or monitoring, or set history or cloud upload to 0. | password manager copies a password → no item, no notification |
| SEN-2 | `store_concealed_from`: list of app identifiers whose concealed content may be stored (empty). Adding one needs encryption at rest on ([STO-4](storage.md)) and a confirmation; items stored this way are flagged `sensitive` (SEN-6). There is no global switch. | encryption off, add an app → refused |
| SEN-3 | Ignore rules: ordered `{name, enabled, app (exact identifier), window_title (regex), text (regex), format (MIME)}`; a change matching every set field of an enabled rule is ignored. A rule with no field set is invalid. | rule window_title `(?i)vault`, copy from "My Vault" → no item |
| SEN-4 | Default rule `password_windows`, enabled: window title matches `(?i)passw(or)?d\|passwort\|mot de passe\|contraseña`. | copy from a window titled "Password Safe" → no item |
| SEN-5 | Blocked texts: "never store this text" on an item deletes it and keeps only a salted SHA-256 of its text; a later copy with the same text is ignored. The text itself is not kept anywhere. Blocked hashes can be listed (by date only) and removed. | block "hunter2", copy it again → no item |
| SEN-6 | Items flagged `sensitive` (by SEN-2 or by the person) show a masked preview (dots plus character count), are matched by search only through note and tags, never appear in notifications, and show masked in the tray menu. | flag an item → list shows "•••• (12 chars)" |
| SEN-7 | After a `sensitive` item is activated, the clipboard is emptied `clear_sensitive_after_s` seconds later (0..3600, default 30; 0 = never), unless the clipboard changed in between. | activate sensitive item, wait 30 s → clipboard empty |
| SEN-8 | Ignored and concealed content leaves no trace: no item, no notification, no log line with its content; the other app's clipboard is left as it is. | copy concealed text, paste elsewhere → still pastes |
| SEN-9 | `hide_from_screen_capture` (default on): the app's windows and menus are excluded from screenshots and screen sharing where the OS supports it (Windows, macOS); elsewhere settings say it has no effect. `show_clipboard_in_title` (default off) controls whether the current clipboard appears in the window title and tray tooltip. | Windows, share screen → app window blank to viewers |
| SEN-10 | Logs contain no item content, window titles or regex texts: only counts, rule names and error reasons. | ignored copy by rule "bank" → log says rule "bank" matched, no content |

## Out of scope

- Detecting secrets by content heuristics (entropy, card numbers).
- Per-item encryption separate from [STO-4](storage.md).

## Decisions

- Concealed content can be stored only per app and only with encryption on (SEN-2); the source allows a global override through scripting.
- Clearing the clipboard after a sensitive paste (SEN-7) is built in; the source offers it as an optional shared script.
