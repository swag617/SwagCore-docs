# Welcome to SwagCore

> **SwagCore** is an Essentials/CMI-style core plugin for Paper/Spigot 1.21+ servers, built on top of **SwagAPI**. It replaces the usual stack of separate homes/warps/moderation/economy plugins with one cohesive core, plus a browser-based admin dashboard.

## What Makes SwagCore Special?

SwagCore is organized into **22 independent modules**, each of which can be toggled on or off in `config.yml` under the `modules:` section. Core systems include:

* **Homes, Warps & Teleportation** — `/home`, `/warp`, `/tpa` with warmups, cooldowns, and a `/back` history
* **Chat** — channel switching, local-radius chat, spam cooldowns, and shadow-muting
* **Moderation** — warn/mute/kick/ban/tempban with duration parsing (`7d`, `1h30m`), staff notes, a staff action log, and alt-account detection
* **Economy** — SwagCore registers itself as the server's **Vault Economy provider** (no third-party economy plugin required — Vault itself must still be installed)
* **Web Dashboard** — a browser admin panel covering players, chat, punishments, economy, leaderboards, a console, and a live tab list/scoreboard editor
* **Identity** — nicknames, pronouns, status messages, and AFK tracking
* **Admin Tools** — vanish, admin mode, invsee, sudo, hat, repair, clear inventory (with confirmation), feed/heal cooldowns, workbench/anvil/enderchest access, and broadcast
* **Rank Progression** — a rank ladder with `/rankup`, distinct from a permissions plugin's rank groups
* **Reports & Playtime** — player-submitted reports for staff review, and playtime milestone tracking
* **Party System** — `/party create|invite|accept|leave|disband|chat`
* **Achievements** — a browsable achievement list
* **Metrics** — `/tps`, `/memory`, `/uptime`, `/plugins`, `/serverinfo` for quick server health checks

## Built on SwagAPI

SwagCore has a **hard dependency on SwagAPI** — it will not enable without it. SwagAPI provides the shared database service, player data service, economy service interface, event bus, messaging service, and (for the dashboard) the shared web service and session-based login system.

Because authentication is handled entirely by SwagAPI's own panel accounts and session cookies, the SwagCore dashboard has **no separate login of its own** — see [Dashboard: Overview & Access](dashboard/overview.md) for details.

## The Web Dashboard

SwagCore's dashboard is registered with SwagAPI's shared `IWebService` and mounted under SwagAPI's web server (typically `http://<swagapi-bind-ip>:<swagapi-port>/swagapi/swagcore/`). It has seven tabs:

| Tab | What it does |
|-----|---------------|
| [Players](dashboard/players.md) | Live table of online players — world, rank, health, gamemode, vanish/AFK status |
| [Chat](dashboard/chat.md) | Recent chat feed plus a broadcast box |
| [Punishments](dashboard/punishments.md) | Last 20 punishments (warns, mutes, kicks, bans) with staff attribution |
| [Economy](dashboard/economy.md) | Circulation total, top balances, and recent transactions |
| [Leaderboards](dashboard/leaderboards.md) | Top balances and most playtime, side by side |
| [Console](dashboard/console.md) | Run console commands from the browser (opt-in, disabled by default) |
| [Editor](dashboard/editor.md) | Live-preview editor for `tablist.yml` and `scoreboard.yml`, with MiniMessage rendering |

## Quick Links

| Topic | Description | Link |
|-------|-------------|------|
| **Installation** | Requirements and first-run setup | [Installation Guide](getting-started/installation.md) |
| **Configuration** | Every `config.yml` key explained | [Configuration](getting-started/configuration.md) |
| **Admin Commands** | Full command reference by category | [Admin Commands](admin-commands.md) |
| **Permissions** | Every permission node and its default | [Permissions](permissions.md) |

## Credits

**Developer:** SwagDev
**Built With:** Java 21, Paper API, SwagAPI, Gson, Vault

## License

SwagCore is proprietary software developed for Swag617 servers.
All rights reserved © 2026

---

> **Need Help?** Check out our [FAQ](troubleshooting/faq.md) or [Support](troubleshooting/support.md) page!
