# Admin Commands

SwagCore registers 100 commands across its 32 modules. This page is the full command reference, organized by system. For the permission node behind each command, see [Permissions](permissions.md).

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
| `/top` | `swagcore.top` | Teleport to the highest solid block above your current position |

`/tpa` requests expire after `teleport.request-timeout-seconds` (default 60s).

## Chat

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ch <channel>` | `swagcore.chat.use` | Switch chat channel |
| `/chatlog <player> [lines]` | `swagcore.chat.log` | View a player's chat log |

Chat spam protection (`chat.spam-cooldown-ms`) and local-radius channel range (`chat.local-radius`) are configured in [Configuration](getting-started/configuration.md). Muted players' messages are shadow-dropped by default (`chat.shadow-mute: true`) rather than rejected — see [`/mute`](#moderation) below.

## Private Messaging & Ignore

| Command | Permission | Description |
|---------|-----------|-------------|
| `/msg <player> <message>` | `swagcore.chat.msg` | Send a private message |
| `/tell <player> <message>` | `swagcore.chat.msg` | Alias of `/msg` |
| `/w <player> <message>` | `swagcore.chat.msg` | Alias of `/msg` |
| `/reply <message>` | `swagcore.chat.msg` | Reply to the last private message you sent or received |
| `/r <message>` | `swagcore.chat.msg` | Alias of `/reply` |
| `/ignore <player\|list>` | `swagcore.ignore` | Ignore or unignore a player's DMs, or list who you're currently ignoring |

**Reply partners.** `/reply` tracks your *current conversation partner* — set on both sides whenever a `/msg` is sent — not just your most recent sender, so replying works correctly even if a third player messaged you in between.

**Ignore blocks incoming DMs, not chat.** If the recipient of a `/msg` is ignoring the sender, the message is silently rejected with a note back to the sender ("... is not accepting messages from you") — this only affects private messages, not public chat channels. Ignore lists persist per player and are cached in memory after the first async load on join.

**Injection-safe.** The message body of a `/msg`/`/reply` is escaped before rendering, so a player can't smuggle `<click>`/`<hover>`/color tags into another player's chat via a DM.

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
| `/ptime <day\|noon\|night\|midnight\|<ticks>\|reset> [player]` | `swagcore.world.ptime` (own) / `swagcore.world.ptime.others` (others) | Set your (or another player's) personal time, independent of the world's actual time |
| `/pweather <clear\|rain\|storm\|reset> [player]` | `swagcore.world.pweather` (own) / `swagcore.world.pweather.others` (others) | Set your (or another player's) personal weather |

Per-world `gamemode`/`fly` rules also live under this module, configured in `worlds.yml` rather than through a command.

## Random Teleport & Sitting

| Command | Permission | Description |
|---------|-----------|-------------|
| `/rtp [world]` | `swagcore.rtp` | Randomly teleport to a safe location, optionally in a specific world |
| `/wild [world]` | `swagcore.rtp` | Alias of `/rtp` |
| `/sit` | `swagcore.sit` | Sit down at your current location, or stand back up if already sitting |

**Safe-landing search.** `/rtp` picks a random point between `rtp.min-radius` and `rtp.max-radius` blocks from world spawn, then checks up to `rtp.max-attempts` times for solid, non-hazardous ground (lava, water, magma, cacti, and fire/campfire blocks are all rejected) with clear air at foot and head height before teleporting. It respects `rtp.cooldown-seconds` (bypassable via `swagcore.rtp.bypasscooldown`), an optional `rtp.cost` charged through Vault, an `rtp.warmup-seconds` delay, and an `rtp.worlds` allow-list (empty = every world allowed).

**Sitting.** `/sit` also triggers passively: right-clicking a stair or slab (when `sit.right-click-blocks` is enabled, the default) sits you on it directly, at the correct height for a top or bottom slab/stair half. Standing up happens automatically on sneak, on taking damage, on dismount, or on quitting — so a sitting player is never stuck.

## Fun & Cosmetic

| Command | Permission | Description |
|---------|-----------|-------------|
| `/scale [player] <factor>` | `swagcore.fun.scale` (self) / `swagcore.fun.scale.others` (others) | Set an entity's model scale, clamped between `0.0625` and `16.0` |
| `/speed [player] <1-10> [walk\|fly]` | `swagcore.fun.speed` (self) / `swagcore.fun.speed.others` (others) | Set walk or fly speed on a 1–10 scale |
| `/nightvision` | `swagcore.fun.nightvision` | Toggle permanent night vision on yourself |
| `/launch [player] [power]` | `swagcore.fun.launch` (self) / `swagcore.fun.launch.others` (others) | Launch a player into the air, power clamped between `0.1` and `5.0` (default `1.5`) |

## Player Toggles & XP Bottling

| Command | Permission | Description |
|---------|-----------|-------------|
| `/toggletpa` | `swagcore.toggle.tpa` | Toggle whether you accept incoming `/tpa`/`/tpahere` requests |
| `/togglepay` | `swagcore.toggle.pay` | Toggle whether you accept incoming `/pay` payments |
| `/xpwithdraw <levels>` | `swagcore.xp.withdraw` | Withdraw XP levels from yourself into a tradeable "Bottled Experience" item |
| `/withdrawxp <levels>` | `swagcore.xp.withdraw` | Alias of `/xpwithdraw` |

Both toggles default to **on** (accepting) and are read by the Teleportation and Economy modules respectively — a player with `toggletpa` off simply can't be targeted by `/tpa`/`/tpahere`. A withdrawn XP bottle is a normal inventory item (right-click to redeem it back into levels), so it can be traded, sold, or stored like any other item — there's no separate deposit command needed.

## Kits

| Command | Permission | Description |
|---------|-----------|-------------|
| `/kit` | `swagcore.kit.use` | Open the kits GUI (same as `/kits`) |
| `/kit <name>` | `swagcore.kit.use` + the kit's own permission | Claim a kit directly by name |
| `/kit create <name> [cooldown-seconds\|once]` | `swagcore.kit.admin` | Save your current inventory as a new (or updated) kit |
| `/kit edit <name>` | `swagcore.kit.admin` | Open the kit editor GUI for an existing kit |
| `/kit delete <name>` | `swagcore.kit.admin` | Delete a kit |
| `/kit list` | `swagcore.kit.use` | Open the kits GUI |
| `/kits` | `swagcore.kit.use` | Open the kits GUI |

Kits are stored in `kits.yml` with Bukkit's native `ItemStack` YAML serialization, so enchantments, lore, and other item meta round-trip correctly. Each kit's per-player permission defaults to `swagcore.kit.<name>` unless a custom `permission` key is set in `kits.yml`. A kit's `cooldown-seconds` can be `0` (no cooldown), a positive number of seconds between claims, or `-1`/`once` for a one-time-only claim ever. Kit commands (run as console on claim) support a `{player}` placeholder.

## Holograms

| Command | Permission | Description |
|---------|-----------|-------------|
| `/hologram create <name>` | `swagcore.holograms.admin` | Create a hologram at your location |
| `/hologram delete <name>` | `swagcore.holograms.admin` | Delete a hologram |
| `/hologram addline <name> <text>` | `swagcore.holograms.admin` | Append a line |
| `/hologram setline <name> <index> <text>` | `swagcore.holograms.admin` | Replace a specific line (1-based index) |
| `/hologram removeline <name> <index>` | `swagcore.holograms.admin` | Remove a specific line |
| `/hologram move <name>` | `swagcore.holograms.admin` | Move a hologram to your location |
| `/hologram tp <name>` | `swagcore.holograms.admin` | Teleport to a hologram |
| `/hologram list` | `swagcore.holograms.admin` | List every hologram by name |
| `/hologram cmiimport` | `swagcore.holograms.admin` | Import holograms directly from `plugins/CMI/Saves/Holograms.yml` |

`/hologram cmiimport` is the standalone equivalent of the Holograms category in the [Migration Assistant](modules/migration.md) — use whichever is more convenient; both skip holograms that already exist by name.

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

## Cross-Server Network

| Command | Permission | Description |
|---------|-----------|-------------|
| `/hub` | `swagcore.network.hub` | Teleport yourself to the network's configured hub server |
| `/send <player> <server>` | `swagcore.network.send` | Send another player to a named server on the network |
| `/network servers` | `swagcore.network.servers` | View registered servers and their cached online counts |

These commands only do anything on a server actually running behind a BungeeCord or Velocity proxy (auto-detected — no config needed to turn detection on) — otherwise they reply that no network is available rather than "Unknown command." See [Cross-Server Network](modules/network.md) for proxy detection, restart evacuation, and the `network:` config block.

## SwagCore Admin Command

| Command | Permission | Description |
|---------|-----------|-------------|
| `/swagcore reload` | `swagcore.admin` | Reload `config.yml` and call `reload()` on every module |
| `/swagcore status` | `swagcore.admin` | Show version, module list, online count, Vault registration status, and any cached plugin updates |
| `/swagcore rules reload` | `swagcore.admin` | Reload `rules.yml` |
| `/swagcore editor [tablist\|scoreboard] [player]` | `swagcore.admin` | Send a clickable link to the web dashboard's Editor tab (console must specify a player) |
| `/swagcore migrate` | `swagcore.admin` | Open the CMI/Essentials [Migration Assistant](modules/migration.md) GUI (player only) |
| `/swagcore migrate skip` | `swagcore.admin` | Permanently dismiss the migration join-prompt without opening the GUI |

All `/swagcore` subcommands require `swagcore.admin` — there's a single top-level permission check, not a per-subcommand one.

## Related Pages

* [Permissions](permissions.md) — every permission node and its default
* [Dashboard Overview & Access](dashboard/overview.md) — the web dashboard, reachable via `/swagcore editor`
* [Migration Assistant](modules/migration.md) — `/swagcore migrate`, importing from CMI/Essentials
* [Cross-Server Network](modules/network.md) — `/hub`, `/send`, `/network servers`, and restart evacuation
* [Configuration](getting-started/configuration.md) — cooldowns, thresholds, and limits referenced throughout this page
