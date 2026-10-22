# Capture

**Status:** Draft · **Prefix:** CAP

## Purpose

Turning changes of the system clipboard into history items: what is stored, what is skipped, and how often.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| CAP-1 | With `store_clipboard` on (default), every clipboard change made by another app becomes an item at the top of the history collection ([COL-1](collections.md)) within 1 s, after the checks of [SEN](sensitive.md) and CAP-3..6. | copy "abc" in an editor → item "abc" on top within 1 s |
| CAP-2 | Stored formats: `text/plain` (as UTF-8), `text/html`, `text/uri-list` (files), `image/png` (platform bitmaps converted to PNG), `image/svg+xml`, `image/gif`, plus any MIME type listed in `extra_formats` (empty). Other formats are dropped; a change with no stored format makes no item. | copy only a private app format → no item |
| CAP-3 | Text that is empty or only whitespace, with no other stored format, makes no item while `ignore_blank` is on (default). | copy "   " → no item |
| CAP-4 | `format_size_limits`: ordered `{pattern (MIME glob), max_bytes}`, first match wins; default one rule `*` = 100 MiB; `max_bytes` 0 never stores that format. An over-limit format is dropped and the rest kept; nothing left → no item. | rule `text/html` 0, copy from a web page → item has text only |
| CAP-5 | Duplicate: when every stored format equals an existing unpinned item of the history collection, that item moves to the top and gets a new `last_used_at`; no item is added. A duplicate of a pinned item adds nothing and moves nothing. | copy "a", "b", "a" → 2 items, "a" on top |
| CAP-6 | Changes the app makes itself (activating an item, [PST-1](paste.md)) create no item; the activated item moves to the top when `move_activated_to_top` is on (default). | activate 3rd item → it is first, count unchanged |
| CAP-7 | Monitoring can be paused and resumed from the tray, a hotkey and the command interface, for 5, 15 or 60 minutes or until resumed. While paused nothing is captured and the tray icon shows the paused state; a timed pause resumes by itself. | pause 5 min, copy → no item; after 5 min, copy → item |
| CAP-8 | Change detection works on every platform within 1 s. On Wayland it needs the compositor's clipboard-access protocol or a desktop extension; without them the app says monitoring is unavailable and stays usable for manual items and paste. | Wayland without protocol → notice, Ctrl+N still works |
| CAP-9 | `notify_on_copy` (default off): a notification on each new item with its first `notification_lines` lines (0..20, default 3) of text, or `image W×H` or the file count; never for content handled by [SEN](sensitive.md). | on, copy 2 files → notification "2 files" |
| CAP-10 | Each captured item records `source_app` and `source_window_title` only while `record_source` is on (default off); otherwise both are used for [SEN-3](sensitive.md) and then discarded. | default, copy → item has no source fields |

## Out of scope

- Capturing drag-and-drop data or the macOS find pasteboard.
- Converting between formats beyond bitmap → PNG.

## Decisions

- Recording the source window is off by default (CAP-10): window titles often reveal document names and are not needed to use history.
- The size limit default of 100 MiB matches the source; the per-format list replaces its environment variable with a setting.
