# Search

**Status:** Draft · **Prefix:** SRC

## Purpose

Finding items quickly by typing.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| SRC-1 | Typing while the item list has focus starts a filter. It matches `text/plain`, the visible text of `text/html` (not tags or attributes), file names in `text/uri-list`, notes and tags. Images and other formats match only through notes and tags. | type "invoice" → item tagged "invoice" shown |
| SRC-2 | Default matching: case-insensitive, and every space-separated word must occur somewhere in the item, in any order. | "foo bar" matches "bar then foo" |
| SRC-3 | `search_regex` (default off): the query is one regular expression, case-insensitive unless `search_case_sensitive` is on; an invalid expression shows its error and no results. | regex `^a\d+$` → only "a12" style items |
| SRC-4 | Results keep collection order; the first result is selected and `Enter` activates it ([PST-1](paste.md)). | filter, Enter → first match on clipboard |
| SRC-5 | `number_search` (default off): typing digits selects that row (1-based) instead of filtering. | on, type 3 → row 3 selected |
| SRC-6 | `Esc` clears the filter; `Esc` with no filter hides the window. | Esc, Esc → filter cleared, window hidden |
| SRC-7 | Scope: current collection by default; `Ctrl+Shift+F` toggles searching all collections, where each result shows its collection name. | scope all → match in "Work" shown with label |
| SRC-8 | Items flagged `sensitive` match only through note and tags ([SEN-6](sensitive.md)). | sensitive item text "pin 1234", search 1234 → not shown |
| SRC-9 | `save_search_history` (default off): keeps the last 20 queries locally, encrypted when [STO-4](storage.md) is on, and cleared by [RET-5](retention.md). | off → no query stored after restart |
| SRC-10 | With 10,000 text items of up to 10 KB each, results update within 300 ms of the last keystroke. | timing test → ≤ 300 ms |

## Out of scope

- Fuzzy or semantic search, OCR of images.

## Decisions

- Searching all collections at once (SRC-7) and the timing target (SRC-10) are designed here; the source filters one collection at a time.
