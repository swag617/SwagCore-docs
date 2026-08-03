# 📦 Installation

## Requirements

### Minimum Requirements
* **Minecraft:** Paper or Spigot 1.21+ (`api-version: '1.21'`)
* **Java:** 21+ (SwagCore is compiled with `--release 21`)
* **Hard dependencies:** [**SwagAPI**](https://github.com/swag617) and **Vault**

SwagCore will refuse to enable if either hard dependency is missing:

* No SwagAPI → `"SwagAPI not found or not enabled! SwagCore cannot start."` and the plugin disables itself.
* No Vault → `"Vault not found! SwagCore requires Vault to provide its economy. Install Vault and restart."` and the plugin disables itself.

### Soft Dependencies (optional)
* **PlaceholderAPI** — placeholder support elsewhere on your server
* **LuckPerms** — recommended permissions plugin
* **SwagFishing** — enables `{fishing_level}`, `{fishing_xp}`, `{fishing_fish_caught}`, `{fishing_prestige}` placeholders in the scoreboard editor
* **SwagFarming** — enables `{farming_level}`, `{farming_crops_harvested}` placeholders in the scoreboard editor

> **You do not need a separate economy plugin.** SwagCore registers itself as the server's Vault Economy provider during `onLoad()`, before other plugins' `onEnable()` runs. Vault itself must still be installed — SwagCore only replaces the economy *provider* behind it.

## Installation Steps

### Step 1: Install SwagAPI and Vault first

SwagCore is a hard dependency on both. Install them, start the server once to confirm they load cleanly, then proceed.

### Step 2: Install SwagCore

1. **Stop your server** (required)
2. Place `SwagCore.jar` in your `plugins/` folder
3. **Start your server**

On first start, SwagCore will:
* Generate `config.yml` and the other default resource files (`chat.yml`, `motd.yml`, `ranks.yml`, `rewards.yml`, `rules.yml`, `scoreboard.yml`, `tablist.yml`, `worlds.yml`, `achievements.yml`, `broadcasts.yml`)
* Initialize its database tables via SwagAPI's shared database service
* Register itself as the Vault Economy provider (check console for `"Registered as the Vault Economy provider."`)
* Lazily create `kits.yml` (on first `/kit create`) and `migration-state.yml` (once the CMI/Essentials join-prompt is shown or `/swagcore migrate` is run) — neither exists until first used

### Step 3: Verify Installation

Run in-game or from console as an operator:

```
/swagcore status
```

You should see something like:

```
=== SwagCore Status ===
Version: 1.0.1
Modules: 32
  ✔ HomesModule
  ✔ WarpsModule
  ...
Online players: 1
SwagAPI: Connected
Economy: registered as the Vault Economy provider
```

## Enabling the Web Dashboard

The dashboard is enabled by default (`dashboard.enabled: true`) and is registered with **SwagAPI's shared web service** — SwagCore does not run its own HTTP server or have its own login page. See [Dashboard: Overview & Access](../dashboard/overview.md) for the full access model.

If you see `"DashboardModule: SwagAPI IWebService not present — dashboard unavailable."` in the console, your SwagAPI installation doesn't have its web server enabled — check SwagAPI's own `config.yml`.

To send a player a clickable link straight to the dashboard (optionally to a specific editor sub-tab):

```
/swagcore editor [tablist|scoreboard] [player]
```

## Restarting vs. Reloading

`config.yml` explicitly warns: **"Restart required after changes — do not use `/reload`."** SwagCore's modules read their settings once at startup; a plugin-manager `/reload` will not pick up new config values reliably.

For most module settings, use:

```
/swagcore reload
```

This reloads `config.yml` and calls `reload()` on every module. For `rules.yml` specifically, use:

```
/swagcore rules reload
```

## File Structure

After installation you'll have:

```
plugins/SwagCore/
├── config.yml           # Main configuration (modules toggle here)
├── chat.yml
├── motd.yml
├── ranks.yml
├── rewards.yml
├── rules.yml
├── scoreboard.yml        # Editable live from the dashboard's Editor tab
├── tablist.yml            # Editable live from the dashboard's Editor tab
├── worlds.yml
├── achievements.yml
├── broadcasts.yml
├── kits.yml               # Created on first /kit create
├── migration-state.yml    # Created once the migration join-prompt fires or /swagcore migrate runs
└── migration-reports/     # One .txt report per completed migration run
```

The SQLite/MySQL database itself is owned and managed by SwagAPI's shared database service, not stored per-plugin.

## Next Steps

* [Configuration](configuration.md) — every `config.yml` key explained
* [Dashboard Overview & Access](../dashboard/overview.md) — how the web dashboard is authenticated and reached
* [Admin Commands](../admin-commands.md) — full command reference
