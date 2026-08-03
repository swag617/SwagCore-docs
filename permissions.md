# Permissions

SwagCore declares 91 permission nodes in `plugin.yml`. All defaults below are exactly as declared — nothing inferred.

## Default Values

* `true` — granted to every player by default. Revoke it from groups that shouldn't have access.
* `op` — granted only to server operators by default.
* `false` — granted to no one by default; must be explicitly assigned (usually to a donor/staff rank).

## Admin

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.admin` | op | Full admin access — also gates every `/swagcore` subcommand and admin-only targeting of other players in several commands |

## Homes & Back

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.homes.use` | true | Use homes |
| `swagcore.homes.set` | true | Set homes |
| `swagcore.homes.admin` | op | Admin homes access |
| `swagcore.homes.1` | true | 1 home slot |
| `swagcore.homes.3` | false | 3 home slots |
| `swagcore.homes.5` | false | 5 home slots |
| `swagcore.homes.10` | false | 10 home slots |
| `swagcore.back` | true | Use `/back` |

The `homes.N` nodes stack as home-slot tiers — grant `swagcore.homes.5` to a donor rank to raise their cap above the `homes.max-homes` config default.

## Warps

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.warps.use` | true | Use warps |
| `swagcore.warps.set` | op | Set warps |
| `swagcore.warps.delete` | op | Delete warps |

## Teleportation

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.tpa` | true | Use `/tpa`, `/tpahere`, `/tpaccept`, `/tpdeny` |
| `swagcore.tp.admin` | op | Admin teleport (`/tp`, `/tpall`, `/tphere`) |

## Chat

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.chat.use` | true | Switch chat channels |
| `swagcore.chat.log` | op | View chat logs (`/chatlog`) |
| `swagcore.chat.color` | false | Use color codes in chat |
| `swagcore.chat.msg` | true | Use `/msg`, `/tell`, `/w`, `/reply`, `/r` |

## Ignore

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.ignore` | true | Use `/ignore` to block incoming private messages from a player |

## Economy

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.economy.balance` | true | View balance |
| `swagcore.economy.pay` | true | Pay other players |
| `swagcore.economy.baltop` | true | View `/baltop` |

## Moderation

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.moderation.warn` | op | Warn players |
| `swagcore.moderation.mute` | op | Mute/unmute players |
| `swagcore.moderation.kick` | op | Kick players |
| `swagcore.moderation.ban` | op | Ban, tempban, and unban players |
| `swagcore.moderation.view` | op | View moderation data — `/checkban`, `/punishments`, `/seen`; also required to receive staff action broadcasts |
| `swagcore.moderation.notes` | op | View/add staff notes |
| `swagcore.moderation.stafflog` | op | View the staff action log |

## Identity

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.identity.nick` | true | Set nickname |
| `swagcore.identity.nick.color` | false | Use color codes in nickname |
| `swagcore.identity.realname` | true | Look up real names from nicknames |
| `swagcore.identity.pronouns` | true | Set pronouns |
| `swagcore.identity.status` | true | Set status message |
| `swagcore.identity.afk` | true | Toggle AFK |

## Admin Tools

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.admin.vanish` | op | Toggle vanish |
| `swagcore.admin.adminmode` | op | Toggle admin mode |
| `swagcore.admin.invsee` | op | View inventories |
| `swagcore.admin.sudo` | op | Force commands on other players |
| `swagcore.admin.hat` | op | Use `/hat` |
| `swagcore.admin.repair` | op | Use `/repair` |
| `swagcore.admin.clearinventory` | op | Use `/ci` / `/clearinventory` |
| `swagcore.admin.feed` | op | Use `/feed` |
| `swagcore.admin.heal` | op | Use `/heal` |
| `swagcore.admin.workbench` | op | Use `/workbench` |
| `swagcore.admin.anvil` | op | Use `/anvil` |
| `swagcore.admin.enderchest` | op | Use `/enderchest` |
| `swagcore.admin.broadcast` | op | Use `/broadcast` |

## World

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.world.spawn` | true | Use `/spawn` |
| `swagcore.world.setspawn` | op | Use `/setspawn` |
| `swagcore.world.ptime` | true | Use `/ptime` on yourself |
| `swagcore.world.ptime.others` | op | Use `/ptime` on other players |
| `swagcore.world.pweather` | true | Use `/pweather` on yourself |
| `swagcore.world.pweather.others` | op | Use `/pweather` on other players |
| `swagcore.top` | true | Use `/top` |

## Random Teleport & Sitting

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.rtp` | true | Use `/rtp` and `/wild` |
| `swagcore.rtp.bypasscooldown` | op | Bypass the `/rtp` cooldown |
| `swagcore.sit` | true | Use `/sit`, and right-click-to-sit on stairs/slabs |

## Fun & Cosmetic

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.fun.scale` | op | Use `/scale` on yourself |
| `swagcore.fun.scale.others` | op | Use `/scale` on other players |
| `swagcore.fun.speed` | op | Use `/speed` on yourself |
| `swagcore.fun.speed.others` | op | Use `/speed` on other players |
| `swagcore.fun.nightvision` | op | Use `/nightvision` |
| `swagcore.fun.launch` | op | Use `/launch` on yourself |
| `swagcore.fun.launch.others` | op | Use `/launch` on other players |

## Player Toggles & XP

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.toggle.tpa` | true | Use `/toggletpa` |
| `swagcore.toggle.pay` | true | Use `/togglepay` |
| `swagcore.xp.withdraw` | true | Use `/xpwithdraw` / `/withdrawxp` |

## Kits

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.kit.use` | true | Claim and browse kits (`/kit`, `/kits`) |
| `swagcore.kit.admin` | op | Create, edit, and delete kits |

> Each kit also has its own claim permission, defaulting to `swagcore.kit.<name>` unless overridden by a `permission` key in `kits.yml`.

## Holograms

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.holograms.admin` | op | Create, edit, and import holograms |

## Cross-Server Network

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.network.hub` | true | Use `/hub` to teleport to the network's hub server |
| `swagcore.network.send` | op | Send a player to another server via `/send` |
| `swagcore.network.servers` | op | View the server/player list of the network via `/network servers` |

## Scoreboard, Profile, Stats & Rank

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.scoreboard.toggle` | true | Toggle personal scoreboard |
| `swagcore.profile.view` | true | View profiles |
| `swagcore.profile.admin` | op | View admin-only profile data |
| `swagcore.stats.view` | true | View your own stats |
| `swagcore.stats.admin` | op | View other players' stats and leaderboards |
| `swagcore.rank.vip` | false | VIP rank tag |
| `swagcore.rank.admin` | op | Admin rank tag |
| `swagcore.rankprogression.rankup` | true | Use `/rankup` and `/rank` (view own) |
| `swagcore.rankprogression.admin` | op | Admin rank management (`/rank set`, `/rank check`) |

> `swagcore.rank.vip` and `swagcore.rank.admin` aren't tied to a command — they're rank-tag permissions consumed by `RankDisplayManager`/`RankConfig` for prefix/display purposes, separate from the `/rankup` ladder progression system.

## Dashboard

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.dashboard.execute` | op | Use the dashboard's [Console tab](dashboard/console.md) execute endpoint (also requires `dashboard.allow-execute: true` in config.yml) |

## Reports & Playtime

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.report.use` | true | Submit player reports |
| `swagcore.reports.view` | op | View and manage the staff report queue |
| `swagcore.playtime.view` | true | View your own playtime |
| `swagcore.playtime.others` | op | View other players' playtime |

## Party & Achievements

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.party.use` | true | Use the party system |
| `swagcore.achievements.view` | true | Browse achievements |

## Metrics

| Permission | Default | Description |
|-----------|---------|-------------|
| `swagcore.metrics.view` | op | View `/tps`, `/memory`, `/uptime`, `/plugins`, `/serverinfo` |

## Commands With No Permission Node

A few commands are declared in `plugin.yml` with an explicitly empty permission (`permission: ""`), meaning **every sender** can run them regardless of any permission plugin:

* `/help [page|category]`
* `/rules`

## Configuring via a Permissions Plugin

```
# Give a group access to moderation view
/lp group staff permission set swagcore.moderation.view true

# Revoke pay access from a muted/restricted group
/lp group restricted permission set swagcore.economy.pay false

# Grant a donor rank 5 home slots
/lp group donor permission set swagcore.homes.5 true
```

## Related Pages

* [Admin Commands](admin-commands.md) — the command each node maps to
* [Dashboard Overview & Access](dashboard/overview.md) — dashboard access is gated by SwagAPI's own panel accounts, not by these permission nodes (with the exception of `swagcore.dashboard.execute`)
* [Migration Assistant](modules/migration.md) — gated entirely by `swagcore.admin`, no dedicated node
* [Cross-Server Network](modules/network.md) — `swagcore.network.hub` / `.send` / `.servers`
