# Items

**Status:** Draft · **Prefix:** ITM

## Purpose

What an item holds, how it is shown, and how the person creates, edits, annotates, pins, deletes and orders items.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| ITM-1 | Item: `{id (UUID), collection_id, position, formats: {MIME type: bytes}, created_at, last_used_at, origin: clipboard\|selection\|manual\|import\|command, pinned (false), sensitive (false), note (≤ 10000 chars), tags (≤ 20, each 1..50 chars, unique case-insensitive)}`. | add 21st tag → refused |
| ITM-2 | Preview: text shows its first `preview_lines` lines (1..50, default 5) with a marker when cut; HTML shows formatted when `rich_text` is on (default), else its text; images show a thumbnail within `preview_image_max` (default 320×240 px) keeping aspect; file lists show names and count; other formats show MIME types and sizes. Note and tags show under the preview. | 12-line text → 5 lines + "more" marker |
| ITM-3 | `Ctrl+N` creates an empty text item at the top of the current collection with `origin: manual` and opens the editor; saving empty text discards it. | Ctrl+N, save empty → no item |
| ITM-4 | `F2` edits text in place; saving replaces `text/plain` and drops `text/html` if the text changed; `Esc` discards the edit. Image-only items cannot be edited in place. | edit an HTML item's text → item has text only |
| ITM-5 | `Ctrl+E` opens the item in `external_editor` (text) or `image_editor` (images) through a temporary file readable only by the user; each save of that file updates the item; the file is deleted when the editor exits. | change text in editor, save → item updated |
| ITM-6 | `Shift+F2` edits the note; tags are added or removed from the item menu. Notes and tags are searchable ([SRC-1](search.md)). | add tag "work" → shown under item |
| ITM-7 | Pin (`Ctrl+Shift+P` toggles): a pinned item keeps its row when items are added or removed above or below it, is never removed by limits ([RET-2](retention.md)) and cannot be deleted until unpinned; the person can still move it. | pin row 3, copy 2 new items → still row 3 |
| ITM-8 | `Delete` removes the selected unpinned items at once; an "undo" offer stays for 10 s, after which the data is purged per [RET-7](retention.md). Selected pinned items are skipped with a notice. | delete 2, undo within 10 s → both back |
| ITM-9 | Order: drag, or `Ctrl+Up`/`Ctrl+Down`/`Ctrl+Home`/`Ctrl+End` moves selected items; `Ctrl+Shift+S` sorts them by text (case-insensitive, locale-aware), `Ctrl+Shift+R` reverses them. Multi-select with Shift/Ctrl-click and `Ctrl+A`. | select 3, Ctrl+Shift+R → order reversed |
| ITM-10 | `F4` shows each format of the item with its MIME type and size and lets the person view it as text or image, or remove one format; removing the last format deletes the item. | remove `text/html` → item keeps text |
| ITM-11 | `sensitive` can be set and cleared by the person on any item ([SEN-6](sensitive.md)). | flag item → preview masked |

## Out of scope

- Rendering arbitrary formats (rich documents, audio, video); they show as format lists.
- Vim-style editing and syntax highlighting.

## Decisions

- Editing text drops the HTML version (ITM-4) so the two never disagree when pasted.
- Pin uses `Ctrl+Shift+P`; the source has no default key for it.
