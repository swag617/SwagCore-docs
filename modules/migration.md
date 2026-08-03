# Migration Assistant

SwagCore ships a GUI-driven, admin-triggered assistant that imports data straight from **CMI** and/or **Essentials** into SwagCore's own tables — homes, warps, economy balances, punishments, nicknames, kits, holograms, and CMI's currency symbol/format. It's part of `MigrationModule`, backed by the classes under `modules/migration/`.

> **Strictly read-only.** The assistant never writes to CMI's or Essentials' files/database — only reads them. That's what makes it safe to run with both plugins still installed, and safe to delete them once you're happy with the result.

## Opening it

```
/swagcore migrate
```

Requires `swagcore.admin` (there's no separate migration permission node — this lives entirely under the `/swagcore` admin command). Console can run `/swagcore migrate skip` to dismiss the join-prompt permanently, but **opening the GUI itself requires a player** — console gets "Console can't open the migration GUI — run this as a player."

## The automatic join-prompt

If a `plugins/CMI/` or `plugins/Essentials/` folder is detected on disk, and migration hasn't already been completed or dismissed, the **first qualifying admin** (`swagcore.admin`) to join after server start sees a one-time chat prompt, about 2 seconds after joining:

```
CMI / Essentials data folder was found. Migrate it into SwagCore?
[Yes, review]   [Don't ask again]
```

* **[Yes, review]** runs `/swagcore migrate` and opens the GUI.
* **[Don't ask again]** runs `/swagcore migrate skip`, which permanently dismisses the prompt (see [Resetting the prompt](#resetting-the-prompt) below) — you can still open the assistant manually at any time.
* The prompt fires **at most once per server boot**, and only to one admin — it won't spam every staff member who logs in.

## The GUI

A 54-slot inventory (`Migration Assistant`) with:

* **Source toggle** (compass icon) — if both a CMI and an Essentials folder are present, click to switch which one you're importing from. Switching resets your category selection to that source's supported categories.
* **Category tiles** — one per category below, showing a live pre-scan count ("Scanning..." until the async scan resolves), a confidence tag, and whether it's currently selected. Categories not supported by the active source render as a gray glass pane and can't be toggled.
* **Select All / Select None** — bulk-select every category the active source supports.
* **Cancel** — closes the GUI without importing anything.
* **Confirm & Migrate** — arms on the first click ("CLICK AGAIN TO CONFIRM", 5-second window), then runs on the second. This double-click guard exists because the action writes to SwagCore's live database.

## Categories & source support

| Category | CMI | Essentials | Confidence |
|----------|:---:|:----------:|------------|
| Homes | ✅ | ✅ | CMI: verified against real data. Essentials: best-effort (no live install to verify format against). |
| Warps | ✅ | ✅ | Same as above |
| Economy Balances | ✅ | ✅ | Same as above |
| Punishments (bans/mutes) | ✅ | ✅ | CMI: bans verified, mutes best-effort (unverified format). Essentials: best-effort overall. |
| Nicknames | ✅ | ✅ | CMI: verified. Essentials: best-effort. |
| Kits | ✅ | ✅ | CMI: items verified, some commands may need manual review. Essentials: best-effort. |
| Holograms | ✅ | ❌ | Essentials has no hologram feature, so it's not offered at all as a source. |
| Currency Symbol/Format | ✅ | ❌ | Essentials has no equivalent of CMI's global currency symbol/placement setting. |

Essentials support is entirely **best-effort** — there was no live Essentials install available to verify the on-disk format against, unlike CMI's categories which were checked against real data (with the two exceptions noted above).

## What each source reads

**CMI** (`plugins/CMI/`):
* `cmi.sqlite.db` — homes, economy balances, punishments, and nicknames all come from CMI's own SQLite user database, read directly via a bundled, relocated `sqlite-jdbc` driver (CMI's own database is a separate physical file from SwagAPI's, so this doesn't touch SwagAPI's connection at all)
* `Saves/Warps.yml`
* `Kits/Kits.yml`
* `Saves/Holograms.yml`
* `config.yml` — for `Economy.Global.CurrencySymbol` (and placement) only

**Essentials** (`plugins/Essentials/`):
* `userdata/` — homes, balances, punishments, nicknames
* `warps/`
* `kits.yml`
* the server-root `banned-players.json` — Essentials' `/ban` delegates to vanilla Bukkit, so the ban list itself lives outside `plugins/Essentials/` entirely

## After migration

A full audit-trail report is written to `plugins/SwagCore/migration-reports/<timestamp>-<SOURCE>.txt`, and a summary posts to chat immediately: imported/skipped/failed counts per category, plus any warnings (e.g. an unresolved hologram whose world isn't loaded). The report explicitly restates that nothing on the source plugin's side was touched.

Kit imports also carry over each kit's console-command list (if any), and imported holograms spawn as real `TextDisplay`-backed holograms managed by SwagCore's own [Holograms](../admin-commands.md#holograms) system — see `/hologram cmiimport` for the standalone version of just that one category.

## Resetting the prompt

The dismiss/complete state lives in its own small file, `plugins/SwagCore/migration-state.yml` (deliberately not in `config.yml`, so it survives config resets). To bring the join-prompt back after dismissing it or completing a migration, stop the server and delete that file — `/swagcore migrate` itself is never blocked by this state either way.

## Related Pages

* [Admin Commands](../admin-commands.md) — the full command reference, including `/swagcore migrate`
* [Holograms](../admin-commands.md#holograms) — `/hologram cmiimport`, the standalone equivalent of the Holograms category
* [Permissions](../permissions.md) — `swagcore.admin` gates the entire `/swagcore` command tree
