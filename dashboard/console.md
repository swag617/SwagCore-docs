# Dashboard: Console

The **Console** tab lets a signed-in staff member run console commands directly from the browser — dispatched as `plugin.getServer().getConsoleSender()`, which is effectively **operator-level access**.

## It's off by default

```yaml
dashboard:
  allow-execute: false
```

With `allow-execute: false` (the default), `POST api/execute` immediately returns:

```json
{"error": "execute endpoint is disabled. Set dashboard.allow-execute: true in config.yml."}
```

with HTTP status `403`, and the dashboard shows this note directly on the tab:

> Requires `dashboard.allow-execute: true` in config.yml.

> **SECURITY:** Console commands run with full operator privileges. Only enable `allow-execute` if the dashboard is not exposed to the public internet — anyone who can reach the dashboard while signed in to a SwagAPI panel account could run arbitrary console commands, including granting themselves permissions or op.

## Using it

Once enabled, type a command (with or without a leading `/`) and press Enter or click **Execute**:

```
POST api/execute
{ "command": "say hello from the dashboard" }
```

The response echoes back whether Bukkit's `dispatchCommand` reported success:

```json
{"executed": true}
```

The output panel prints `> <command>` followed by `[OK]` (green) or `[FAILED]` (red). Every executed command is also logged server-side at `INFO` level, tagged with the signed-in panel username:

```
[Dashboard] <username> executed console command: /say hello from the dashboard
```

## Related Pages

* [Dashboard Overview & Access](overview.md) — how the signed-in username used in the log line is resolved
* [Permissions](../permissions.md) — `swagcore.dashboard.execute`
* [Configuration](../getting-started/configuration.md) — the `dashboard:` config block
