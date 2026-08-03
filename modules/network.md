# Cross-Server Network

`NetworkModule` adds three BungeeCord/Velocity-aware commands — `/hub`, `/send`, and `/network servers` — plus automatic **restart evacuation**, for servers that run behind a proxy as part of a multi-server network.

## Proxy auto-detection

The module checks, once at startup:

* `spigot.yml` → `settings.bungeecord`
* `config/paper-global.yml` → `proxies.velocity.enabled`

If neither is set, this server isn't behind a proxy. The three commands **still register** rather than disappearing — running any of them just replies:

> This server isn't connected to a network right now.

...instead of Bukkit's generic "Unknown command." This is expected and harmless on a standalone server.

## How it talks to the proxy

All communication rides the legacy `BungeeCord` plugin-messaging channel. Velocity answers this same channel through its own built-in BungeeCord-compatibility layer, so **no proxy-side plugin is required** on either BungeeCord or Velocity, and there's no dependency on any particular hub-side plugin (e.g. SwagHub) being installed. There is also no handshake to confirm what plugin — if any — is running on the other end; a successful response only confirms the proxy is reachable and, for `/network servers`, that a server by that name is registered on it.

## Commands

| Command | Permission | Default | Description |
|---------|-----------|---------|-------------|
| `/hub` | `swagcore.network.hub` | true | Teleport yourself to the configured hub server |
| `/send <player> <server>` | `swagcore.network.send` | op | Send another (possibly offline-here) player to a named server |
| `/network servers` | `swagcore.network.servers` | op | List cached servers and their last-known online counts |

`/network servers` reads from a cache refreshed by a periodic poll (`network.poll-interval-seconds`, default 30s) — the poll only fires while at least one player is online, and only after a proxy was detected. If the proxy hasn't answered yet, it says so rather than showing stale/empty data as if it were current.

## Configuration

```yaml
network:
  hub-server: "hub"
  poll-interval-seconds: 30

  # --- Restart evacuation reporting (optional) ---
  this-server-name: ""
  hub-url: ""
  shared-secret: ""
```

| Key | Default | Description |
|-----|---------|-------------|
| `hub-server` | `"hub"` | The proxy-registered server name `/hub` sends players to |
| `poll-interval-seconds` | `30` | How often the server list/player counts refresh in the background |
| `this-server-name` | *(blank)* | This server's own name as registered on the proxy — only needed for restart auto-return |
| `hub-url` | *(blank)* | Base URL of the hub's SwagHub network-service mount, e.g. `http://<hub ip>:<port>/swagnet/swaghub/` |
| `shared-secret` | *(blank)* | Must match `network.shared-secret` in both this server's and the hub's SwagAPI `config.yml` |

## Restart evacuation

When SwagRestartScheduler fires a restart on this server, it publishes a `server.restart.pending` event on SwagAPI's shared event bus. `RestartEvacuationListener` subscribes to that event and, as long as a proxy was detected:

1. Sends **every online player** to `network.hub-server` immediately via the same BungeeCord channel `/hub` uses.
2. If `this-server-name`, `hub-url`, and `shared-secret` are **all** set, best-effort POSTs the evacuated player list (`uuid` + `name`) to `<hub-url>/evacuated` with an `X-SwagNetwork-Key` header, so the hub can send everyone back automatically once this server finishes rebooting.

Leaving any of those three keys blank **only skips the auto-return reporting** — evacuation to the hub still happens either way. A failed report (timeout, unreachable hub, etc.) is logged as a warning and never undoes or blocks the evacuation itself. If no proxy was detected, or no players are online when the event fires, nothing happens.

## Server-to-server API

Independent of the proxy detection above, `NetworkModule` also registers a small HTTP handler (`NetworkApiHandler`) with SwagAPI's shared web service, mounted at `/swagnet/swagcore/` and authenticated by SwagAPI's own shared-secret service auth (not the human dashboard's session cookie). It exposes one route, `GET /player/<uuid>`, returning a small curated JSON summary (balance, rank, playtime, home count) — so a hub-style server can display a player's stats without needing its own database connection into this server's tables. This only requires SwagAPI's web service to be present; it works even on a server with no BungeeCord/Velocity proxy at all.

## Related Pages

* [Admin Commands](../admin-commands.md) — full command reference
* [Permissions](../permissions.md) — `swagcore.network.hub` / `.send` / `.servers`
* [Configuration](../getting-started/configuration.md) — the rest of `config.yml`
