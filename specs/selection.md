# Selection

**Status:** Draft · **Prefix:** SYN

## Purpose

Where the OS has a primary selection next to the clipboard, storing selected text and keeping the two in step.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| SYN-1 | These rules apply only where a primary selection exists: Linux X11, and Wayland compositors with the primary-selection protocol. Elsewhere the settings are hidden. | Windows → no selection settings |
| SYN-2 | `store_selection` (default off): selection changes also become items, with `origin: selection`, through the same checks as [CAP](capture.md) and [SEN](sensitive.md). | on, select "foo" → item "foo" |
| SYN-3 | Growing selection: when the new selection text starts or ends with the text of the top item and that item came from the selection, the top item is updated instead of adding one. | select "he", extend to "hello" → one item "hello" |
| SYN-4 | When the clipboard receives the same text as the top selection item, the clipboard's formats are merged into that item instead of adding one. | select "x", then copy "x" → one item with html too |
| SYN-5 | `sync_clipboard_to_selection` and `sync_selection_to_clipboard` (both default off) copy the text of one to the other after it changes. Only text formats are copied. | clipboard → selection on, copy "a" → middle-click pastes "a" |
| SYN-6 | A sync waits until no Shift, Ctrl or Alt key is held (giving up after 5 s) and at least 50 ms after the change, and is skipped when the source changed again meanwhile or the target already holds the same text. | hold Shift while selecting → no sync until released |
| SYN-7 | Content ignored or concealed by [SEN](sensitive.md) is never synchronised. | concealed clipboard → selection unchanged |
| SYN-8 | On Wayland without the needed protocols the settings show as unavailable with the reason. | protocol missing → options greyed with reason |

## Out of scope

- The macOS find pasteboard and other secondary buffers.

## Decisions

- Sync is off in both directions by default; each one silently overwrites data, so the person turns it on knowingly.
