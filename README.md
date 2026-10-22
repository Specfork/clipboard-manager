---
specfork: 1
name: "@specfork/clipboard-manager"
summary: Local desktop clipboard history for text, HTML, images and files, with limits, search, pins, collections, paste, global hotkey, ignore rules for secrets, editing, optional encryption and retention.
license: CC-BY-4.0
derived_from:
  repo: https://github.com/hluk/CopyQ
  commit: 9d202b6fb8b977a5fad565c8dcb500f460af3ebe
  url: https://github.com/hluk/CopyQ
  license: GPL-3.0-or-later
---

# Clipboard manager

One person keeps a history of what they copy on their own computer. Every copy of text, formatted
text, an image or a list of files becomes an item; items can be searched, edited, pinned, sorted
into collections and pasted back into the app the person was using. Content that the source app or
the person marks as secret is never stored, history can be capped, aged out, cleared or encrypted,
and nothing leaves the device.
Behaviour derived from CopyQ's open-source code and documentation (GPL-3.0-or-later), written fresh and stack-free.
Not affiliated with or endorsed by Lukas Holecek (hluk) or the CopyQ project. The name is used only to credit the source.

| Area | Prefix | Covers |
|---|---|---|
| [capture.md](specs/capture.md) | CAP | Clipboard monitoring, stored formats, size limits, duplicates, pause |
| [sensitive.md](specs/sensitive.md) | SEN | Concealed marker, ignore rules, blocked texts, masking, screen-capture protection, logs |
| [selection.md](specs/selection.md) | SYN | Primary selection: storing, merging, synchronising with the clipboard |
| [items.md](specs/items.md) | ITM | Item fields, previews, create, edit, notes, tags, pin, delete, move, sort |
| [collections.md](specs/collections.md) | COL | History and other collections, favourites, moving items between them |
| [search.md](specs/search.md) | SRC | Filtering items, matching rules, regex, scope |
| [paste.md](specs/paste.md) | PST | Activating items, pasting into the previous window, plain text, tray menu, popup |
| [retention.md](specs/retention.md) | RET | Count limits, age limits, clearing, history kept in memory only, delete all |
| [storage.md](specs/storage.md) | STO | Local files, saving, encryption at rest, lock, export and import |
| [app.md](specs/app.md) | APP | Background instance, tray, hotkeys, window, shortcuts, command interface, settings |

Single user, no account, no server, no network. Runs on Windows, macOS and Linux (X11 and Wayland);
platform differences are stated as behaviour. Shortcuts are defaults and configurable; `Ctrl` means
`Cmd` on macOS. Formats are named by MIME type; instants are UTC ISO 8601; setting and field names
are `snake_case`.

This spec is licensed under Creative Commons Attribution 4.0 International (CC BY 4.0):
https://creativecommons.org/licenses/by/4.0/legalcode

It describes behaviour of CopyQ (https://github.com/hluk/CopyQ, GPL-3.0-or-later)
in new words; no code or text was copied.
See README.md front matter `derived_from`.
