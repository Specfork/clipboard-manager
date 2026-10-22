# Retention

**Status:** Draft · **Prefix:** RET

## Purpose

How much history is kept, for how long, and how the person removes it.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| RET-1 | `max_items` 1..100000 (default 200) per collection, overridable per collection ([COL-1](collections.md)). Adding an item to a full collection removes the bottom-most unpinned item. | limit 3, add 4th → oldest unpinned gone |
| RET-2 | Pinned items are never removed by limits. When every item is pinned and the collection is full, the new item is not added and a notice says why, once per hour at most. | 3 pinned, limit 3, copy → no item, notice |
| RET-3 | Lowering `max_items` below the current count asks for confirmation naming how many items will be removed, then trims at once. | 200 items, set 50 → "150 items will be removed" |
| RET-4 | `max_age_days` 0..3650 (default 0 = off): unpinned items of the history collection whose `last_used_at` is older are removed at start and every hour. Other collections are never aged. | 30 days, item last used 31 days ago → removed |
| RET-5 | Clear history: removes all unpinned items of the history collection and the search history after confirmation; a checkbox in the same confirmation also removes pinned ones. | clear → only pinned remain |
| RET-6 | `clear_on_exit` (default off): unpinned history items are removed on quit, and at next start if the app did not quit cleanly. | on, kill app, start → history empty |
| RET-7 | Removal is real: a removed item's data, including separately stored large formats, is gone from storage at the next save ([STO-3](storage.md)); there is no hidden trash. | remove image item, inspect data folder → its file gone |
| RET-8 | `persist_history` (default on): when off, the history collection is held in memory only and never written to disk; other collections are still saved. | off, copy, quit, start → history empty, no history file |
| RET-9 | Delete all data: after typing `DELETE`, every collection, item, ignore rule, blocked hash and search query is removed and the history collection recreated empty; settings stay. | delete all → one empty "Clipboard" |

## Out of scope

- Size-based quotas for a whole collection (bytes); limits are per format ([CAP-4](capture.md)) and per item count.

## Decisions

- Default limit 200 items matches the source; age limits and memory-only history are designed here.
- `@specfork/privacy` is not required: it depends on `@specfork/auth`, which this account-free app does not use; delete-all (RET-9) and export (STO-10) follow the same intent locally.
