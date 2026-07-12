# Dashboard: Punishments

The **Punishments** tab lists the **20 most recent punishments** issued through SwagCore's moderation system, newest first.

## Columns

| Column | Notes |
|--------|-------|
| Type | Color-coded badge — red for `BAN`, yellow for `MUTE`, gray for everything else (`WARN`, `KICK`, `TEMPBAN`) |
| Target | Resolved player name if available, otherwise the raw target UUID |
| Staff | Resolved staff name if available, otherwise the raw staff UUID |
| Reason | The reason text given when the punishment was issued |
| Date | Localized full date/time, rendered client-side from the stored millisecond timestamp |

## Data source

`GET api/punishments` reads from the `swagcore_punishments` table:

```sql
SELECT id, target_uuid, staff_uuid, type, reason, timestamp
FROM swagcore_punishments ORDER BY timestamp DESC LIMIT 20
```

Target and staff names are resolved server-side via `Bukkit.getOfflinePlayer(UUID)` after the query — if a name can't be resolved (e.g. the UUID was never seen by this server), the raw UUID is shown instead.

## Where punishments come from

Every punishment issued via the in-game commands ([`/warn`](../admin-commands.md), [`/mute`](../admin-commands.md), [`/kick`](../admin-commands.md), [`/ban`](../admin-commands.md), [`/tempban`](../admin-commands.md)) is written to the same `swagcore_punishments` table this tab reads from — the dashboard is a read-only view of that history, not a separate punishment system. There is currently no way to issue a punishment *from* the dashboard itself; use the in-game/console commands or `/punishments <player>`'s in-game GUI.

## Related Pages

* [Admin Commands](../admin-commands.md) — the full moderation command reference, including duration syntax for `/mute` and `/tempban`
* [Permissions](../permissions.md) — `swagcore.moderation.*` nodes
