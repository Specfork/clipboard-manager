# App

**Status:** Draft · **Prefix:** APP

## Purpose

The background instance, tray, global hotkeys, main window, keyboard shortcuts, the minimal command interface and settings.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| APP-1 | One background instance per user session. Starting the app again shows the first instance's window and exits with code 0. | start twice → one process, window shown |
| APP-2 | Tray icon: primary click toggles the main window, or opens the tray menu when `tray_click` is `menu`; the menu has recent items ([PST-9](paste.md)), pause/resume ([CAP-7](capture.md)), show/hide, clear history, settings and quit. Without a tray area the app relies on hotkeys and says so once. | click tray → window toggles |
| APP-3 | Global hotkeys, configurable: show popup ([PST-8](paste.md)) `Ctrl+Alt+V` (macOS `Cmd+Option+V`); show/hide main window, pause/resume and `paste_item_n` unset. A hotkey held by another app or the OS is marked `not registered`; the rest still work. | another app holds Ctrl+Alt+V → marked |
| APP-4 | Wayland: hotkeys go through the desktop's global-shortcut service when present (the desktop may ask to confirm); otherwise settings show the `toggle` and `menu` commands ([APP-7](app.md)) to bind in the desktop's own settings. | Wayland without service → commands shown |
| APP-5 | Main window: the current collection's items, collection bar, filter. It opens on the display with the pointer, keeps size and position where the platform allows, hides on `Esc`, and hides 500 ms after losing focus when `close_on_unfocus` is on (default). | click elsewhere → hidden after 0.5 s |
| APP-6 | Window shortcuts, configurable, defaults: `Enter` activate, `Shift+Enter` plain text, `F2` edit, `Shift+F2` note, `Ctrl+E` external editor, `F4` formats, `Delete` remove, `Ctrl+N` new item, `Ctrl+T` new collection, `Ctrl+W` delete collection, `Ctrl+Shift+P` pin, `Ctrl+Up/Down` move, `Ctrl+,` settings, `Ctrl+Q` quit. Assigning a key already in use is refused with the action that holds it. | assign F2 to pin → refused, names edit |
| APP-7 | Command interface: a CLI talks to the running instance over a per-user local channel, never a network port: `show`, `hide`, `toggle`, `menu`, `add <text>…`, `read [row] [--format <MIME>]`, `select <row>` (activate without paste), `remove <row>…`, `count`, `pause [minutes]`, `resume`, `clear`, each optionally after `--collection <name>`. Rows are 0-based from the top. | `add a b`, `count` → 2 |
| APP-8 | Command exit codes: 0 done, 1 no running instance, 2 usage error or no such row/collection, 3 locked ([STO-6](storage.md)). `add` goes through [SEN-3](sensitive.md) rules like a copy. Requests from another OS user are refused. | locked, `read 0` → exit 3 |
| APP-9 | `start_at_login` (default off) is offered once on first run and adds or removes a per-user login item without administrator rights. | accept on first run → starts after next login |
| APP-10 | Settings live in one per-user file; unknown keys are ignored and invalid values fall back to their default, each with a warning, and the rest still load. Changes apply without restart. | `max_items=-4` → 200 used, warning |
| APP-11 | `ui_language` (auto = OS language, falls back to English); `theme: system\|light\|dark` (system); `confirm_quit` (default on) asks before quitting because capture stops. | quit → confirmation shown |

## Out of scope

- Scripting language, plugins, automatic commands triggered by clipboard content, menus of custom actions; only APP-7 commands exist.
- Self-update and installers.

## Decisions

- A minimal command set (APP-7) replaces the source's full scripting engine, which is out of scope.
- The show hotkey defaults to `Ctrl+Alt+V`, avoiding `Ctrl+Shift+V`, which many apps use to paste as plain text.
