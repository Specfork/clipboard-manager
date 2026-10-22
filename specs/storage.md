# Storage

**Status:** Draft · **Prefix:** STO

## Purpose

Where items live on the device, how they are saved safely, optional encryption at rest with locking, and moving data in and out.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| STO-1 | All data stays in the per-user OS data folder, created readable and writable only by that user; the app makes no network requests at all. | inspect permissions → owner-only; traffic monitor → zero requests |
| STO-2 | Format data larger than `inline_max_bytes` (default 4096) is stored in its own file referenced by the item and deleted with it. | copy 1 MB image → separate file exists |
| STO-3 | Changes are saved within 2 s; every write goes to a temporary file that replaces the old one only when complete, so a crash loses at most the last 2 s and never corrupts a collection. | kill app mid-save → previous state loads |
| STO-4 | `encrypt` (default off): all collection files, separate format files, search history and blocked hashes are encrypted with authenticated encryption (AES-256-GCM or XChaCha20-Poly1305) under a random 256-bit data key; the data key is wrapped by a key derived from the person's password (Argon2id ≥ 64 MiB, ≥ 3 passes). Settings and ignore rules stay readable. | encrypt on, search data folder for a copied word → not found |
| STO-5 | Turning encryption on asks for a password (≥ 8 chars, typed twice), says a forgotten password makes the data unrecoverable, rewrites all data encrypted and deletes the plain files. Turning it off needs the password and rewrites plain. | turn on → old plain files gone |
| STO-6 | Locked state (at start with encryption on, or after STO-7): items are not shown, monitoring is paused and nothing is captured, the tray shows locked; entering the password unlocks. | locked, copy → no item after unlock |
| STO-7 | `lock_after_min` 0..1440 (default 0 = never): with encryption on, after that many minutes without interaction with the app, the data key is dropped from memory and the app locks. | 5 min idle → password needed |
| STO-8 | `use_os_keystore` (default off): the data key is kept in the OS credential store so start needs no password; STO-7 still applies. | on, restart → unlocked without prompt |
| STO-9 | Changing the password needs the old one and re-wraps only the data key, taking under 1 s at any history size. Five wrong passwords in a row add a delay of 30 s, doubling each further failure up to 1 h; data is never deleted for failures. | 6th wrong try → wait 60 s |
| STO-10 | Export: chosen collections or all into one file `{format: "clipboard-manager-export", version: 1, exported_at, collections: [{name, kind, items: [{formats (base64), created_at, last_used_at, pinned, note, tags, sensitive}]}]}`; sensitive items are left out unless chosen; an optional export password encrypts the whole file (STO-4 scheme). | export 3 items → file has 3 items |
| STO-11 | Import of such a file adds its items as new items (new ids) into collections of the same name, created when missing; nothing existing is overwritten; limits ([RET-1](retention.md)) apply; a wrong password or another `format`/newer `version` → `unsupported_export` and nothing changes. | import twice → items twice, originals untouched |

## Out of scope

- Sync between devices, cloud backup, sharing collections.

## Decisions

- Save within 2 s (STO-3); the source batches saves for minutes, which loses recent history on a crash.
- The OS key store is off by default (STO-8), so encrypted data needs the password after restart; the source turns it on by default.
- Locking pauses capture (STO-6) rather than queueing plaintext in memory.
