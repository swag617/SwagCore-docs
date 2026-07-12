# Dashboard: Players

The **Players** tab is the dashboard's default landing tab. It shows a live table of every player currently online.

## Columns

| Column | Source |
|--------|--------|
| Name | `Player#getName()` |
| World | `Player#getWorld()#getName()` |
| Rank | `RankDisplayManager#getRankIdForPlayer(player)` |
| Health | `Player#getHealth()`, shown to 1 decimal place |
| Mode | `Player#getGameMode()` |
| Status | badges — see below |

## Status badges

* **Vanished** (gray badge) — shown if `AdminModule#isVanished(uuid)` is true (see [`/vanish`](../admin-commands.md))
* **AFK** (yellow badge) — read from `IdentityModule`'s in-memory AFK cache
* **Online** (green badge) — shown only when neither of the above applies

A vanished *and* AFK player shows both badges together.

## Behavior notes

* The table is fetched via `GET api/players` and only reflects players online **at the moment the tab loads or is refreshed** — it does not auto-poll.
* Data collection happens on the main server thread (via `Bukkit.getScheduler().runTask`), since it touches live `Player` objects; the HTTP response is completed asynchronously once that task finishes.
* If no one is online, the table shows a single centered "No players online" row instead of an empty `<tbody>`.

## Related Pages

* [Dashboard Overview & Access](overview.md)
* [Admin Commands](../admin-commands.md) — `/vanish`, `/adminmode`, and other staff tools referenced here
