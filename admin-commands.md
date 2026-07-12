# Admin Commands

SwagCore registers 74 commands across its 22 modules. This page is the full command reference, organized by system. For the permission node behind each command, see [Permissions](permissions.md).

## Help

| Command | Permission | Description |
|---------|-----------|-------------|
| `/help [page\|category]` | *(open to all)* | Browse SwagCore commands |

## Homes & Warps

| Command | Permission | Description |
|---------|-----------|-------------|
| `/home [name]` | `swagcore.homes.use` | Teleport to a home |
| `/sethome [name]` | `swagcore.homes.set` | Set a home at your location |
| `/delhome [name]` | `swagcore.homes.set` | Delete a home |
| `/homes [player]` | `swagcore.homes.use` | Open the homes GUI |
| `/back [number]` | `swagcore.back` | Teleport to your last location (or a specific entry in your `/back` history) |
| `/warp <name>` | `swagcore.warps.use` | Warp to a location |
| `/setwarp <name>` | `swagcore.warps.set` | Set a warp at your location |
| `/delwarp <name>` | `swagcore.warps.delete` | Delete a warp |
| `/warps` | `swagcore.warps.use` | Open the warps GUI browser |

Home slots beyond the `homes.max-homes` config default are granted via the `swagcore.homes.1` / `.3` / `.5` / `.10` permission nodes. `/home`, `/warp`, and `/tpa` all apply a warmup (`homes.warmup-seconds`, `warps.warmup-seconds`, `teleport.warmup-seconds`) before teleporting.

## Teleportation

| Command | Permission | Description |
|---------|-----------|-------------|
| `/tpa <player>` | `swagcore.tpa` | Request to teleport to a player |
| `/tpahere <player>` | `swagcore.tpa` | Request a player to teleport to you |
| `/tpaccept` | `swagcore.tpa` | Accept a teleport request |
| `/tpdeny` | `swagcore.tpa` | Deny a teleport request |
| `/tp <player> [target]` | `swagcore.tp.admin` | Teleport to a player (admin, bypasses request/accept flow) |
| `/tpall` | `swagcore.tp.admin` | Teleport every online player to you |
| `/tphere <player>` | `swagcore.tp.admin` | Teleport a player to you |

`/tpa` requests expire after `teleport.request-timeout-seconds` (default 60s).

## Chat

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ch <channel>` | `swagcore.chat.use` | Switch chat channel |
| `/chatlog <player> [lines]` | `swagcore.chat.log` | View a player's chat log |

Chat spam protection (`chat.spam-cooldown-ms`) and local-radius channel range (`chat.local-radius`) are configured in [Configuration](getting-started/configuration.md). Muted players' messages are shadow-dropped by default (`chat.shadow-mute: true`) rather than rejected — see [`/mute`](#moderation) below.

## Economy

| Command | Permission | Description |
|---------|-----------|-------------|
| `/balance [player]` | `swagcore.economy.balance` | View a balance (your own, or another player's if you also hold `swagcore.admin`) |
| `/balance history` | `swagcore.economy.balance` | View your own last 10 transactions |
| `/bal [player]` | `swagcore.economy.balance` | Alias of `/balance` |
| `/pay <player> <amount>` | `swagcore.economy.pay` | Pay another online player |
| `/baltop` | `swagcore.economy.baltop` | View the top 10 balances server-wide |

**Payment confirmation.** Payments at or above `economy.pay-confirm-threshold` (default `10000.0`) don't execute immediately — the player is prompted to run `/pay confirm` to finalize. If `economy.daily-pay-limit` is set (non-zero), payments are also checked against the player's running daily total before the confirmation prompt, and rejected outright if they'd exceed the limit.

**Vault provider.** SwagCore registers itself as the server's Vault Economy provider on `onLoad()` — no separate economy plugin is required, but Vault itself must be installed. See [Configuration](getting-started/configuration.md#economy).

## Moderation

| Command | Permission | Description |
|---------|-----------|-------------|
| `/warn <player> <reason>` | `swagcore.moderation.warn` | Warn a player |
| `/mute <player> [duration] <reason>` | `swagcore.moderation.mute` | Mute a player, permanently or for a duration |
| `/unmute <player>` | `swagcore.moderation.mute` | Unmute a player |
| `/kick <player> [reason]` | `swagcore.moderation.kick` | Kick a player (defaults to "Kicked by an admin" if no reason given) |
| `/ban <player> [-e <url>] <reason>` | `swagcore.moderation.ban` | Permanently ban a player, with an optional evidence link |
| `/tempban <player> <duration> [-e <url>] <reason>` | `swagcore.moderation.ban` | Temporarily ban a player, with an optional evidence link |
| `/unban <player>` | `swagcore.moderation.ban` | Unban a player (clears both `BAN` and `TEMPBAN`) |
| `/checkban <player>` | `swagcore.moderation.view` | Check whether a player is currently banned |
| `/punishments <player>` | `swagcore.moderation.view` | Open a GUI showing a player's full punishment history |
| `/seen <player>` | `swagcore.moderation.view` | View a player's first-join and last-seen timestamps |
| `/notes <player> [add <note>]` | `swagcore.moderation.notes` | View or add a staff note on a player |
| `/stafflog [page]` | `swagcore.moderation.stafflog` | View the paginated staff action log |

**Duration syntax.** `/mute` and `/tempban` durations are parsed by a regex that accepts chained `<number><unit>` pairs, e.g. `30s`, `5m`, `1h`, `7d`, `2w`, or combined like `1h30m`. If `/mute`'s second argument doesn't parse as a duration, it's treated as the start of the reason and the mute becomes permanent. `/tempban` requires a valid duration and rejects the command with `"Invalid duration. Use e.g. 7d, 1h30m."` otherwise.

**Evidence links.** `/ban` and `/tempban` accept an optional `-e <url>` flag immediately after the player name (and duration, for `/tempban`) to attach an evidence link, shown back to the issuing staff member and stored with the punishment.

**Shadow mute.** Muting a player also flips a shadow-mute flag in `ChatModule` — by default (`chat.shadow-mute: true`) their messages are silently dropped rather than met with an error, so they may not immediately realize they're muted.

**Staff notifications.** `/warn`, `/kick`, and `/ban` broadcast a `[Staff] ...` message to every online player holding `swagcore.moderation.view`.

## Identity

| Command | Permission | Description |
|---------|-----------|-------------|
| `/nick <nickname\|reset>` | `swagcore.identity.nick` | Set your nickname (`swagcore.identity.nick.color` additionally allows color codes) |
| `/realname <nickname>` | `swagcore.identity.realname` | Look up the real name behind a nickname |
| `/pronouns <he/him\|she/her\|they/them\|custom>` | `swagcore.identity.pronouns` | Set your pronouns |
| `/status <message>` | `swagcore.identity.status` | Set your status message (capped at `identity.max-status-length`, default 64 chars) |
| `/afk` | `swagcore.identity.afk` | Toggle AFK status manually |

Players are also auto-marked AFK after `identity.afk-timeout-ticks` (default 6000 ticks = 5 minutes) of inactivity.

## Admin & Utility

| Command | Permission | Description |
|---------|-----------|-------------|
| `/vanish [player]` | `swagcore.admin.vanish` | Toggle vanish (targeting another player requires `swagcore.admin`) |
| `/adminmode` | `swagcore.admin.adminmode` | Toggle admin mode — creative, flight, invulnerability, auto-vanish, and a red boss bar |
| `/invsee <player>` | `swagcore.admin.invsee` | View a player's live inventory |
| `/sudo <player> <command>` | `swagcore.admin.sudo` | Force a player to run a command |
| `/hat` | `swagcore.admin.hat` | Wear the held item as a helmet |
| `/repair [all]` | `swagcore.admin.repair` | Repair the held item, or the whole inventory with `all` |
| `/ci [player]` | `swagcore.admin.clearinventory` | Clear an inventory (self-clear requires confirmation) |
| `/clearinventory [player]` | `swagcore.admin.clearinventory` | Alias of `/ci` |
| `/feed [player]` | `swagcore.admin.feed` | Restore hunger (cooldown: `admin.feed-cooldown-seconds`) |
| `/heal [player]` | `swagcore.admin.heal` | Restore health (cooldown: `admin.heal-cooldown-seconds`) |
| `/workbench` | `swagcore.admin.workbench` | Open a virtual crafting table |
| `/anvil` | `swagcore.admin.anvil` | Open a virtual anvil |
| `/enderchest [player]` | `swagcore.admin.enderchest` | Open your (or, with `swagcore.admin`, another player's) ender chest |
| `/broadcast <message>` | `swagcore.admin.broadcast` | Broadcast a formatted `[Broadcast]` message server-wide |

**Clear-inventory confirmation.** Running `/ci` with no arguments (or targeting yourself) stages a self-clear and asks you to run `/ci confirm` within 30 seconds. Targeting *another* player (`/ci <player>`) clears immediately with no confirmation step, and notifies the target.

**Admin mode.** Enabling admin mode automatically vanishes you (if not already vanished) and shows a persistent red `ADMIN MODE ACTIVE` boss bar. Disabling it reverts gamemode to survival, removes flight/invulnerability, and un-vanishes you.

**Console can broadcast too.** `/broadcast` is the one admin command console can run directly — every other command in this table requires an in-game player.

## World

| Command | Permission | Description |
|---------|-----------|-------------|
| `/spawn [world]` | `swagcore.world.spawn` | Teleport to spawn |
| `/setspawn [world]` | `swagcore.world.setspawn` | Set the world spawn |

## Scoreboard

| Command | Permission | Description |
|---------|-----------|-------------|
| `/scoreboard toggle` | `swagcore.scoreboard.toggle` | Toggle your personal scoreboard visibility |

Scoreboard *content* (title/lines, including per-world overrides) is edited in `scoreboard.yml` or live via the dashboard's [Editor tab](dashboard/editor.md), not through this command.

## Profile & Stats

| Command | Permission | Description |
|---------|-----------|-------------|
| `/profile [player]` | `swagcore.profile.view` | View a player profile |
| `/stats [player\|leaderboard <key>]` | `swagcore.stats.view` | View your own stats, or (with `swagcore.stats.admin`) another player's stats and leaderboards |

## Rank Progression

| Command | Permission | Description |
|---------|-----------|-------------|
| `/rankup` | `swagcore.rankprogression.rankup` | Rank up to the next rank on the ladder |
| `/rank [set <player> <rankId>\|check <player>]` | `swagcore.rankprogression.rankup` (base) / `swagcore.rankprogression.admin` (set/check others) | View rank info, or admin-manage a player's rank |

This is a **rank ladder progression system** (earn your way up), distinct from the rank *groups* your permissions plugin (e.g. LuckPerms) manages.

## Rules & Reports

| Command | Permission | Description |
|---------|-----------|-------------|
| `/rules` | *(open to all)* | View the server rules |
| `/report <player> <reason>` \| `/report history` | `swagcore.report.use` | Submit a player report, or view your own report history |
| `/reports` | `swagcore.reports.view` | View and manage the open staff report queue |

`/report` submissions are rate-limited by `reports.max-per-hour` (default 3). Bump `rules.version` in `config.yml` and run `/swagcore rules reload` whenever `rules.yml` content changes, to force everyone to re-accept.

## Playtime, Party & Achievements

| Command | Permission | Description |
|---------|-----------|-------------|
| `/playtime [player]` | `swagcore.playtime.view` (own) / `swagcore.playtime.others` (others) | View playtime and milestone progress |
| `/party <create\|invite\|accept\|leave\|disband\|chat>` | `swagcore.party.use` | Party system |
| `/pc <message>` | `swagcore.party.use` | Send a message to your party chat |
| `/achievements` | `swagcore.achievements.view` | Browse achievements |

Party size is capped by `party.max-size` (default 6).

## Server Metrics

| Command | Permission | Description |
|---------|-----------|-------------|
| `/tps` | `swagcore.metrics.view` | View server TPS |
| `/memory` | `swagcore.metrics.view` | View JVM memory usage |
| `/uptime` | `swagcore.metrics.view` | View server uptime |
| `/plugins` | `swagcore.metrics.view` | List loaded plugins |
| `/serverinfo` | `swagcore.metrics.view` | View combined server metrics (TPS + memory + uptime + player count) |

## SwagCore Admin Command

| Command | Permission | Description |
|---------|-----------|-------------|
| `/swagcore reload` | `swagcore.admin` | Reload `config.yml` and call `reload()` on every module |
| `/swagcore status` | `swagcore.admin` | Show version, module list, online count, Vault registration status, and any cached plugin updates |
| `/swagcore rules reload` | `swagcore.admin` | Reload `rules.yml` |
| `/swagcore editor [tablist\|scoreboard] [player]` | `swagcore.admin` | Send a clickable link to the web dashboard's Editor tab (console must specify a player) |

All `/swagcore` subcommands require `swagcore.admin` — there's a single top-level permission check, not a per-subcommand one.

## Related Pages

* [Permissions](permissions.md) — every permission node and its default
* [Dashboard Overview & Access](dashboard/overview.md) — the web dashboard, reachable via `/swagcore editor`
* [Configuration](getting-started/configuration.md) — cooldowns, thresholds, and limits referenced throughout this page
