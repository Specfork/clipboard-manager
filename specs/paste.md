# Paste

**Status:** Draft · **Prefix:** PST

## Purpose

Getting an item back into the app the person is working in: activation, pasting, plain text, several items, the tray menu and the popup.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| PST-1 | Activate (`Enter`, double-click, or single click when `activate_on_single_click` is on, default off) puts every stored format of the item on the clipboard, updates its `last_used_at` and moves it per [CAP-6](capture.md). | activate HTML item, paste in a rich editor → formatted |
| PST-2 | `activate_pastes` (default on): after activation the window hides, focus returns to the window active before the app opened, and the paste keystroke is sent. `activate_pastes` off: copy only; `activate_hides` (default on) controls hiding. | text editor focused, popup, Enter → text inserted |
| PST-3 | Paste keystroke: `Ctrl+V` (macOS `Cmd+V`) by default; `paste_key_rules`: ordered `{app or window_title regex, keys}` override it, e.g. `Ctrl+Shift+V` or `Shift+Insert` for terminals. | rule terminal → `Ctrl+Shift+V` sent there |
| PST-4 | If the previous window no longer exists or cannot take focus, the item is only copied and a notice says so. | close target app, activate → copied, notice |
| PST-5 | Wayland: when sending key events to other apps is not permitted, the app copies only and says "copied, paste with Ctrl+V", unless the desktop offers a consented input service. macOS: sending the paste needs the Accessibility permission; without it, copy only and explain how to grant it. | macOS without permission → copied, hint shown |
| PST-6 | `Shift+Enter` activates as plain text: only `text/plain` is put on the clipboard. | Shift+Enter on HTML item → paste is unformatted |
| PST-7 | Several selected items activate as one: their texts joined by `item_separator` (default newline) in list order, text only. Selected items without text are skipped. | select "a","b", Enter → "a\nb" |
| PST-8 | Popup: the show hotkey ([APP-3](app.md)) opens a compact list of the history collection at the pointer (Wayland: centre of the active display), focused on the filter; `Enter`, a click or digits `1`..`9` (first nine rows) activate. | hotkey, press 2 → second item pasted |
| PST-9 | Tray menu lists the first `tray_items` (0..99, default 5) items of the history collection with one-line previews (masked when `sensitive`); choosing one activates it, pasting only when `tray_pastes` is on (default on). | 5 items in tray menu, choose 3rd → pasted |
| PST-10 | Optional hotkeys `paste_item_1`..`paste_item_9` (unset by default) activate and paste that row of the history collection without opening any window. | bind paste_item_2, press → 2nd item pasted |

## Out of scope

- Typing text key by key into apps that block paste.
- Templates or placeholders expanded on paste.

## Decisions

- Pasting into the previous window is the default (PST-2), as in the source; platforms that forbid it fall back to copy with a clear hint (PST-5) instead of failing silently.
