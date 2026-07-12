# Dashboard: Economy

The **Economy** tab gives a snapshot of the server economy — total circulation, top balances, and the most recent transactions.

## Total Circulation

Computed with a single query against the `swagcore_transactions` table:

```sql
SELECT SUM(CASE WHEN type='DEPOSIT' OR type='REWARD' THEN amount ELSE 0 END)
     - SUM(CASE WHEN type='WITHDRAW' OR type='PAY' THEN amount ELSE 0 END) AS circulation
FROM swagcore_transactions
```

This is a **proxy** for circulation based on logged transaction history, not a live sum of every player's Vault balance (which would require iterating every offline player).

## Top Balances

The 10 highest balances among **currently online players**, read live via the Vault economy service (`plugin.getSwagAPI().getEconomyService()`) and sorted descending. Offline players are not included — this table only reflects who's online right now.

## Recent Transactions

The 20 most recent rows from `swagcore_transactions`, newest first:

```sql
SELECT uuid, amount, type, reason, timestamp FROM swagcore_transactions ORDER BY timestamp DESC LIMIT 20
```

Each row shows a color-coded badge (`DEPOSIT`/`REWARD` in green, everything else — `WITHDRAW`/`PAY` — in red), the formatted amount, an optional reason string, and the timestamp.

## Related Pages

* [Leaderboards](leaderboards.md) — a persistent top-balances view that isn't limited to online players
* [Admin Commands](../admin-commands.md) — `/balance`, `/pay`, `/baltop`
* [Configuration](../getting-started/configuration.md) — the `economy:` config block (pay confirmation threshold, daily pay limit, currency name)
