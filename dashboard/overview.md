# Dashboard: Overview & Access

SwagCore ships a browser-based admin dashboard covering seven tabs: [Players](players.md), [Chat](chat.md), [Punishments](punishments.md), [Economy](economy.md), [Leaderboards](leaderboards.md), [Console](console.md), and the [Tab List & Scoreboard Editor](editor.md).

## How it's hosted

SwagCore does **not** run its own `HttpServer`. Instead, `DashboardModule` registers a single handler with **SwagAPI's shared `IWebService`**:

```
webService.registerModule(plugin, new DashboardHttpHandler(plugin));
```

The dashboard is reachable at:

```
http://<swagapi-bind-ip>:<swagapi-port>/swagapi/swagcore/
```

If `dashboard.enabled` is `false` in `config.yml`, or SwagAPI's web service isn't present at startup, the dashboard is skipped entirely and a warning is logged — `"DashboardModule: SwagAPI IWebService not present — dashboard unavailable."`

## Authentication

There is **no separate dashboard login**. Authentication is handled entirely by SwagAPI's shared session-cookie system, before any request ever reaches SwagCore's handler:

* SwagAPI gates the `/swagapi/swagcore/` mount point with its own login screen and session cookie.
* The dashboard's frontend (`app.js`) calls every API endpoint through an `apiFetch()` helper that sets `credentials: 'include'`.
* If a request comes back `401` or `302` (session missing/expired), the frontend redirects to `/login?redirect=<current-path>`.
* The "Signed in as X" indicator and the live TPS/RAM/Players/Uptime chips in the top bar come from a shared include, `/swagapi/shared/topbar.js` — not from SwagCore itself.
* Server-side, `DashboardApiHandler` resolves the signed-in username via `webService.getSessionUsername(exchange)` for attribution in logs and chat broadcasts (falling back to the literal string `"dashboard"` if unavailable).

To manage who can sign in at all, see SwagAPI's own `config.yml` under `web-server.auth` — panel accounts are a SwagAPI concept shared by every plugin registered on its web service, not something SwagCore configures.

## Opening the dashboard from in-game

```
/swagcore editor [tablist|scoreboard] [player]
```

Sends a clickable chat link that opens the dashboard directly on the Editor tab (optionally straight to the Tab List or Scoreboard sub-tab). Console must specify a player name; a player running it with no arguments gets a link sent to themselves. This uses the same deep-link query parameters (`?tab=editor&sub=tablist`) the dashboard's `app.js` reads on page load — see [Editor](editor.md).

## Requirements

* `dashboard.enabled: true` in `config.yml` (default)
* SwagAPI's web service must be enabled and reachable
* The signed-in SwagAPI panel account needs whatever permission your SwagAPI setup requires to view plugin panels; SwagCore's own `swagcore.dashboard.execute` permission additionally gates the [Console tab's](console.md) execute endpoint

## Related Pages

* [Players](players.md) · [Chat](chat.md) · [Punishments](punishments.md) · [Economy](economy.md) · [Leaderboards](leaderboards.md) · [Console](console.md) · [Editor](editor.md)
* [Configuration](../getting-started/configuration.md) — the `dashboard:` config block
