# Collections

**Status:** Draft · **Prefix:** COL

## Purpose

Grouping items into named collections: the history that receives copies, favourites, and any others the person makes.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| COL-1 | Collection: `{id, name (1..100 chars, trimmed, unique case-insensitive), position, kind: history\|regular, max_items (optional, overrides [RET-1](retention.md))}`. Exactly one collection is `history` (default name "Clipboard"); captures go there. | fresh install → one collection "Clipboard" |
| COL-2 | Another collection can be made the history; the previous one becomes regular and keeps its items. | make "Work" history → next copy lands in "Work" |
| COL-3 | `Ctrl+T` creates a collection; rename from its menu; `Ctrl+W` deletes the current one after a confirmation naming its item count. The history collection cannot be deleted. | Ctrl+W on history → refused |
| COL-4 | With `collection_tree` on (default off), `/` in names groups collections in a tree: "Work/Snippets" shows under "Work". | create "A/B" → tree node A with child B |
| COL-5 | The collection bar is hidden while only one collection exists; with `show_counts` on (default off) each collection shows its item count. | create second collection → bar appears |
| COL-6 | Items move or copy between collections by drag (`Ctrl` held = copy) or the item menu; formats, note, tags and `sensitive` travel along, `pinned` becomes false, and the target's limit applies ([RET-1](retention.md)). | move pinned item → arrives unpinned |
| COL-7 | Favourites: "add to favourites" copies the item into the collection named by `favourites_collection` (default "Favourites"), created on first use; adding an equal item again does nothing. | add twice → one copy in Favourites |
| COL-8 | Navigation: `Ctrl+Tab` / `Ctrl+Shift+Tab` next and previous, `Ctrl+1`..`Ctrl+9` the nth collection, `Ctrl+0` the last. | Ctrl+2 → second collection shown |

## Out of scope

- Collections backed by a folder on disk or a cloud folder.
- Automatic sorting of new items into collections by rules or scripts.

## Decisions

- Favourites are a plain collection (COL-7), so no separate data model is needed.
