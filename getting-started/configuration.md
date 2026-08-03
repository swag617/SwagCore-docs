# Configuration

SwagCore uses a single `config.yml` at `plugins/SwagCore/config.yml`. It documents every section below, in the same order they appear in the file.

> **Restart required after changes — do not use `/reload`.** Use `/swagcore reload` instead (see [Installation](installation.md)).

---

## Modules

```yaml
modules:
  homes: true
  warps: true
  teleport: true
  chat: true
  economy: true
  moderation: true
  identity: true
  admin: true
  world: true
  motd: true
  scoreboard: true
  tablist: true
  profile: true
  stats: true
  dashboard: true
  rankprogression: true
  rules: true
  reports: true
  playtime: true
  achievements: true
  party: true
  metrics: true
  holograms: true
  rtp: true
  sit: true
  fun: true
  toggles: true
  xp: true
  kits: true
  messaging: true
  ignore: true
  network: true
  migration: true
```

Every one of SwagCore's 32 modules can be independently disabled by setting its value to `false`. Disabling a module skips its command registration and listeners entirely.

---

## Homes

```yaml
homes:
  max-homes: 3
  warmup-seconds: 3
  cooldown-seconds: 30
  back-history-size: 5
```

| Key | Default | Description |
|-----|---------|-------------|
| `max-homes` | `3` | Base number of homes a player may set. Permission nodes (`swagcore.homes.1/3/5/10`) can grant more. |
| `warmup-seconds` | `3` | Delay before a `/home` teleport executes |
| `cooldown-seconds` | `30` | Cooldown between `/home` uses |
| `back-history-size` | `5` | How many previous locations `/back <number>` can recall |

---

## Warps

```yaml
warps:
  warmup-seconds: 3
```

| Key | Default | Description |
|-----|---------|-------------|
| `warmup-seconds` | `3` | Delay before a `/warp` teleport executes |

---

## Teleport

```yaml
teleport:
  warmup-seconds: 3
  request-timeout-seconds: 60
```

| Key | Default | Description |
|-----|---------|-------------|
| `warmup-seconds` | `3` | Delay before a `/tpa`/`/tpahere` teleport executes once accepted |
| `request-timeout-seconds` | `60` | How long a `/tpa` request stays valid before expiring |

---

## Random Teleport

```yaml
rtp:
  min-radius: 200
  max-radius: 3000
  max-attempts: 20
  cooldown-seconds: 60
  warmup-seconds: 3
  cost: 0.0
  worlds: []
```

| Key | Default | Description |
|-----|---------|-------------|
| `min-radius` / `max-radius` | `200` / `3000` | Block-distance range from world spawn `/rtp` searches within |
| `max-attempts` | `20` | How many random points to try before giving up with "Couldn't find a safe location" |
| `cooldown-seconds` | `60` | Cooldown between `/rtp` uses (bypassable via `swagcore.rtp.bypasscooldown`) |
| `warmup-seconds` | `3` | Delay before the teleport executes once a safe spot is found |
| `cost` | `0.0` | Vault charge per use. `0` = free |
| `worlds` | `[]` (empty) | Allow-list of world names `/rtp` is permitted in. Empty = every world allowed |

---

## Sit

```yaml
sit:
  right-click-blocks: true
```

| Key | Default | Description |
|-----|---------|-------------|
| `right-click-blocks` | `true` | Right-click a stair or slab to sit on it, in addition to the plain `/sit` command |

---

## Chat

```yaml
chat:
  local-radius: 100
  spam-cooldown-ms: 1500
  shadow-mute: true
```

| Key | Default | Description |
|-----|---------|-------------|
| `local-radius` | `100` | Block radius for the local chat channel |
| `spam-cooldown-ms` | `1500` | Minimum milliseconds between chat messages per player |
| `shadow-mute` | `true` | When true, muted players' messages are silently dropped rather than blocked with an error (they don't realize they're muted) |

---

## Economy

```yaml
economy:
  pay-confirm-threshold: 10000.0
  daily-pay-limit: 0.0
  starting-balance: 0.0
  currency-singular: "Coin"
  currency-plural: "Coins"
```

| Key | Default | Description |
|-----|---------|-------------|
| `pay-confirm-threshold` | `10000.0` | Payments at or above this amount require running `/pay confirm` to finalize |
| `daily-pay-limit` | `0.0` | Maximum a player can send via `/pay` per day. `0` = no limit |
| `starting-balance` | `0.0` | Balance new players start with |
| `currency-singular` | `"Coin"` | Singular currency name (Vault display) |
| `currency-plural` | `"Coins"` | Plural currency name (Vault display) |

> SwagCore is its own Vault Economy provider — no third-party economy plugin is needed. Vault itself must still be installed for other plugins to see this economy.

---

## Moderation

```yaml
moderation:
  alt-detection: true
```

| Key | Default | Description |
|-----|---------|-------------|
| `alt-detection` | `true` | Enable alt-account detection for staff |

---

## Identity

```yaml
identity:
  afk-timeout-ticks: 6000
  max-status-length: 64
```

| Key | Default | Description |
|-----|---------|-------------|
| `afk-timeout-ticks` | `6000` | Ticks of inactivity (6000 = 5 minutes) before a player is auto-marked AFK |
| `max-status-length` | `64` | Maximum character length for `/status <message>` |

---

## Admin

```yaml
admin:
  feed-cooldown-seconds: 30
  heal-cooldown-seconds: 30
```

| Key | Default | Description |
|-----|---------|-------------|
| `feed-cooldown-seconds` | `30` | Cooldown on the staff `/feed` command |
| `heal-cooldown-seconds` | `30` | Cooldown on the staff `/heal` command |

---

## Scoreboard

```yaml
scoreboard:
  update-interval-ticks: 20
  enabled-by-default: true
```

| Key | Default | Description |
|-----|---------|-------------|
| `update-interval-ticks` | `20` | How often the scoreboard refreshes (20 ticks = 1 second) |
| `enabled-by-default` | `true` | Whether new players see the scoreboard by default |

The scoreboard's actual title/line content lives in `scoreboard.yml`, editable live from the dashboard's [Editor tab](../dashboard/editor.md).

---

## Tab List

```yaml
tablist:
  update-interval-ticks: 40
  sort: RANK_WEIGHT
```

| Key | Default | Description |
|-----|---------|-------------|
| `update-interval-ticks` | `40` | How often the tab list header/footer refresh (40 ticks = 2 seconds) |
| `sort` | `RANK_WEIGHT` | Tab list player sort mode |

The tab list's header/footer content lives in `tablist.yml`, editable live from the dashboard's [Editor tab](../dashboard/editor.md).

---

## Dashboard

```yaml
dashboard:
  enabled: true
  allow-execute: false
```

| Key | Default | Description |
|-----|---------|-------------|
| `enabled` | `true` | Register the dashboard with SwagAPI's shared web service |
| `allow-execute` | `false` | Allow `POST /api/execute` (running console commands from the [Console tab](../dashboard/console.md)) |

> **SECURITY:** only set `allow-execute: true` if the dashboard is not exposed to the public internet — commands run as console (effectively OP). Access at `http://<swagapi-bind-ip>:<swagapi-port>/swagapi/swagcore/`. Login is handled entirely by SwagAPI's own panel accounts (see SwagAPI's `config.yml` `web-server.auth` section) — SwagCore has no separate dashboard password. See [Dashboard Overview & Access](../dashboard/overview.md).

---

## Rules

```yaml
rules:
  version: 1
```

| Key | Default | Description |
|-----|---------|-------------|
| `version` | `1` | Bump this whenever `rules.yml` content changes, to force everyone to re-accept the rules |

---

## Reports

```yaml
reports:
  max-per-hour: 3
```

| Key | Default | Description |
|-----|---------|-------------|
| `max-per-hour` | `3` | Maximum `/report` submissions a single player can make per hour |

---

## Party

```yaml
party:
  max-size: 6
```

| Key | Default | Description |
|-----|---------|-------------|
| `max-size` | `6` | Maximum members in a single party |

---

## Metrics

```yaml
metrics:
  # TPS color thresholds and sample buffer are fixed; nothing to configure yet.
```

The metrics module (`/tps`, `/memory`, `/uptime`, `/plugins`, `/serverinfo`) currently has no configurable options.

---

## Network

```yaml
network:
  hub-server: "hub"
  poll-interval-seconds: 30
  this-server-name: ""
  hub-url: ""
  shared-secret: ""
```

| Key | Default | Description |
|-----|---------|-------------|
| `hub-server` | `"hub"` | Proxy-registered server name `/hub` sends players to |
| `poll-interval-seconds` | `30` | How often the cached server list/player counts refresh |
| `this-server-name` | *(blank)* | This server's own proxy-registered name — only needed for restart auto-return |
| `hub-url` | *(blank)* | Base URL of the hub's SwagHub network-service mount — only needed for restart auto-return |
| `shared-secret` | *(blank)* | Must match `network.shared-secret` in both this server's and the hub's SwagAPI `config.yml` |

`/hub`, `/send`, and `/network servers` only activate if this server is actually behind a BungeeCord/Velocity proxy — auto-detected from `spigot.yml`'s `settings.bungeecord` or `config/paper-global.yml`'s `proxies.velocity.enabled`, no config needed to turn detection on. The three keys below `poll-interval-seconds` are entirely optional and only affect **restart-evacuation auto-return** — leaving them blank still evacuates players to the hub on restart, they just won't be sent back automatically. See [Cross-Server Network](../modules/network.md).

---

## Migration Assistant

The Migration Assistant (`/swagcore migrate`) has no `config.yml` section of its own — its one piece of state (whether the join-prompt has been completed or dismissed) lives in a separate `plugins/SwagCore/migration-state.yml`, not in `config.yml`, so it survives config resets. See [Migration Assistant](../modules/migration.md).

---

## Applying Changes

```
/swagcore reload
```

Reloads `config.yml` and calls `reload()` on every enabled module. For `rules.yml` specifically:

```
/swagcore rules reload
```

Do **not** use the server's generic `/reload` — SwagCore explicitly documents this as unsupported in `config.yml`'s header comment.
