# Frequently Asked Questions

## Installation & Startup

**SwagCore won't enable — the console says "SwagAPI not found or not enabled!"**

SwagCore has a hard dependency on SwagAPI. Install SwagAPI, confirm it enables cleanly on its own, then restart with SwagCore present. See [Installation](../getting-started/installation.md).

---

**SwagCore won't enable — the console says "Vault not found!"**

Install the [Vault](https://www.spigotmc.org/resources/vault.34315/) plugin. You do **not** need a separate economy plugin behind it — SwagCore registers itself as the Vault Economy provider automatically. See [Configuration: Economy](../getting-started/configuration.md#economy).

---

**I changed config.yml — do I need to restart?**

`config.yml` itself says: *"Restart required after changes — do not use `/reload`."* In practice, run `/swagcore reload` first — it reloads config and calls `reload()` on every module. If a setting doesn't seem to apply after that, a full restart is the safe fallback. `rules.yml` has its own reload command: `/swagcore rules reload`.

---

## Dashboard

**The dashboard says the page can't be reached.**

Check three things, in order:
1. `dashboard.enabled: true` in `config.yml` (it's the default)
2. SwagAPI's web service is actually running — check SwagAPI's own `config.yml` for its web-server settings
3. Console for `"DashboardModule: SwagAPI IWebService not present — dashboard unavailable."` — if you see this, the dashboard didn't register at all

The dashboard is normally reachable at `http://<swagapi-bind-ip>:<swagapi-port>/swagapi/swagcore/`. See [Dashboard Overview & Access](../dashboard/overview.md).

---

**I get redirected to `/login` when opening the dashboard.**

That's expected — the dashboard has no login of its own. You need a valid SwagAPI panel account and an active session. Sign in there first; SwagCore's dashboard trusts SwagAPI's session cookie entirely.

---

**The Console tab says the execute endpoint is disabled.**

That's the default. Set `dashboard.allow-execute: true` in `config.yml` and restart (or `/swagcore reload`) to enable it. Read the security warning in [Dashboard: Console](../dashboard/console.md) first — console commands run with full operator privileges.

---

**The Editor tab flags a placeholder I typed as "unknown."**

The tab list editor only recognizes `{online}` `{max}` `{date}` `{time}` `{tps}`. The scoreboard editor additionally recognizes `{world}` `{balance}` `{rank}` and the SwagFishing/SwagFarming/jobs placeholders listed in [Dashboard: Editor](../dashboard/editor.md). A flagged placeholder still saves fine — the warning doesn't block you — but it also won't resolve to anything meaningful in-game unless the required plugin is installed and the name is exactly right.

---

## Moderation

**How do I write a mute/ban duration?**

Chain `<number><unit>` pairs with no separator: `30s`, `5m`, `1h`, `7d`, `2w`, or combinations like `1h30m`. See [Admin Commands: Moderation](../admin-commands.md#moderation).

---

**Why didn't my `/mute` become temporary?**

If the second argument to `/mute` doesn't parse as a valid duration, SwagCore treats the entire remainder as the reason and mutes permanently instead of erroring. Double-check your duration syntax if you meant it to be temporary.

---

## Economy

**Why does `/pay` ask me to run `/pay confirm`?**

Payments at or above `economy.pay-confirm-threshold` (default `10000.0`) require a confirmation step to prevent costly typos. Run `/pay confirm` right after to finalize the staged payment.

---

**Why is `/baltop` (or the dashboard's top-balances list) missing an offline player I know is rich?**

The dashboard's Economy and Leaderboards tabs read top balances **live from online players only** — `/baltop` (the command) queries all players including offline ones, so use that instead if you need a true server-wide ranking. See [Dashboard: Leaderboards](../dashboard/leaderboards.md).

---

## Migration

**The migration join-prompt never showed up.**

It only fires once per server boot, to the first online player holding `swagcore.admin`, and only if a `plugins/CMI/` or `plugins/Essentials/` folder actually exists on disk. If you dismissed it before (or already completed a migration), it won't ask again — run `/swagcore migrate` manually any time, or delete `plugins/SwagCore/migration-state.yml` and restart to bring the prompt back. See [Migration Assistant](../modules/migration.md).

---

**Is it safe to run the migration assistant while CMI/Essentials are still installed?**

Yes — it's strictly read-only against CMI's and Essentials' own files/database. Nothing on the source plugin's side is ever modified, so you can migrate, review the results, and only remove the old plugin once you're satisfied.

---

## Network

**`/hub`, `/send`, and `/network servers` say "This server isn't connected to a network right now."**

This is expected on a standalone server — these commands only activate once a BungeeCord or Velocity proxy is detected (`spigot.yml`'s `settings.bungeecord` or `config/paper-global.yml`'s `proxies.velocity.enabled`). If you *are* running behind a proxy and still see this, double-check those settings are actually enabled and the server has restarted since. See [Cross-Server Network](../modules/network.md).

---

**Players weren't sent back after a scheduled restart.**

Evacuation to the hub itself doesn't need any config — it works as soon as a proxy is detected. Auto-*return*, however, requires `network.this-server-name`, `network.hub-url`, and `network.shared-secret` to all be set and matching the hub's own SwagAPI config. Leave any of the three blank and evacuation still happens, it just won't report back for an automatic return trip.

---

## General

**Where is my data stored?**

SwagCore doesn't manage its own database file — it uses SwagAPI's shared database service. Punishments, transactions, and chat logs live in tables like `swagcore_punishments`, `swagcore_transactions`, and `swagcore_chat_log` within that shared database.

---

**Need more help?**

See the [Support](support.md) page.
