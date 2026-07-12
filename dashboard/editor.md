# Dashboard: Tab List & Scoreboard Editor

The **Editor** tab is SwagCore's most involved dashboard feature: a live-preview visual editor for `tablist.yml` and `scoreboard.yml`, with two sub-tabs — **Tab List** and **Scoreboard**.

## Shared building blocks

Both editors work on the same underlying concept: a list of **frames** (strings in MiniMessage format) that cycle on a configurable tick delay, giving animated headers/footers/titles.

* Every frame input is validated live against a known placeholder list for that field — unrecognized `{placeholder}` tokens are flagged in red with a warning underneath, without blocking saving.
* A **Live Preview** panel renders the MiniMessage-lite markup (colors, `<bold>`, `<italic>`, `<underlined>`, `<strikethrough>`, `<obfuscated>`, hex colors, `<gradient:...>`, `<rainbow>`) against mock placeholder values, and cycles between frames on the same delay you configured — a close approximation of what players will actually see. `<click>`/`<hover>`/`<insert>` and other interactive tags are recognized but ignored (their content still renders; the tag itself has no visual effect in the preview).
* Obfuscated text is animated in the preview with a scrambling effect, refreshed every 90ms.

## Tab List sub-tab

Backed by `GET`/`POST api/editor/tablist`, which reads/writes `tablist.yml`'s `header` and `footer` sections (`frames` list + `frame-delay-ticks`, defaulting to 15 for the header and 20 for the footer).

**Recognized placeholders:** `{online}` `{max}` `{date}` `{time}` `{tps}`

The preview shows a mock two-column player list (Notch, Steve, Alex / jeb_, Herobrine) with your header/footer rendered above and below it.

Saving calls `TabListModule#saveAndReload`, which writes the YAML and reloads the module immediately — **no restart required**, and no separate `/swagcore reload` needed either.

## Scoreboard sub-tab

Backed by `GET`/`POST api/editor/scoreboard`, which reads/writes `scoreboard.yml`'s `default` section plus any per-world `worlds.<name>` overrides. Each section has a `title` (frames + delay, default 10 ticks) and a flat `lines` list (no per-line animation).

**Recognized placeholders:** `{online}` `{max}` `{date}` `{time}` `{world}` `{balance}` `{rank}` `{fishing_level}` `{fishing_xp}` `{fishing_fish_caught}` `{fishing_prestige}` `{farming_level}` `{farming_crops_harvested}` `{job}` `{job_level}`

> The `fishing_*` placeholders only resolve in-game if SwagFishing is installed; `farming_*` require SwagFarming; `job`/`job_level` require a jobs integration. The editor still lets you type them (they're on the known list so they won't be flagged), but the live preview always uses mock values — it can't verify a soft-dependency plugin is actually installed. `%papi%`-style PlaceholderAPI placeholders aren't validated in the editor at all; they're only resolved in-game.

### Per-world overrides

Use the **Editing section** dropdown to switch between the `default` fallback and any world-specific override. **+ Add world override** prompts for a world name — it must match the Bukkit world folder name exactly (a hint lists currently-loaded world names, fetched from `GET api/editor/worlds`). **Delete override** removes a world's override and falls back to `default` (with a confirmation prompt).

Saving calls `ScoreboardModule#saveAndReload` — writes `scoreboard.yml` and applies it live, same as the tab list.

## Opening a specific sub-tab from in-game

```
/swagcore editor tablist [player]
/swagcore editor scoreboard [player]
```

sends a link with `?tab=editor&sub=tablist` (or `sub=scoreboard`) — the dashboard's `app.js` reads these query params on load and jumps straight to that sub-tab.

## Related Pages

* [Dashboard Overview & Access](overview.md) — how `/swagcore editor` links are generated and authenticated
* [Configuration](../getting-started/configuration.md) — the `scoreboard:` and `tablist:` config blocks (update intervals, sort mode)
