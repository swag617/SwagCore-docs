# Dashboard: Leaderboards

The **Leaderboards** tab shows two side-by-side ranked lists, both fetched from a single `GET api/leaderboards` call.

## Top Balances

Same source and limitations as the [Economy tab's](economy.md) top-balances list: the top 10 balances among **online players only**, read live from the Vault economy service and sorted descending. This is *not* an all-time/all-players leaderboard — a wealthy offline player won't appear here.

## Most Playtime

The top 10 players by total playtime, sourced from `StatsModule`'s stored `PLAYTIME_TICKS` stat (persists across sessions, unlike the balance list — this one does include offline players). Ticks are converted to `Xh Ym` for display (20 ticks = 1 second).

If the `stats` module is disabled in `config.yml`, this list is simply empty rather than erroring.

## Related Pages

* [Economy](economy.md) — total circulation and recent transactions
* [Admin Commands](../admin-commands.md) — `/stats`, `/playtime`, `/baltop`
