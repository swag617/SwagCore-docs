# Support

## Getting Help

The fastest way to get help with SwagCore is the **Swag617 Discord** — server staff and the developer are both reachable there.

[Join the Discord](https://discord.gg/9rKuThh6yU)

Before asking, check:
1. The [FAQ](faq.md) — covers the most common issues
2. The relevant documentation page for your topic (use the search bar in the sidebar)

## Reporting a Bug

When reporting a bug in Discord, include:

* Your server version (e.g., Paper 1.21.4 build 123)
* SwagCore version — check `/swagcore status`
* SwagAPI version, since SwagCore depends on it directly
* A clear description of what happened vs. what you expected
* Relevant console errors or stack traces (paste the full error, not just the first line)
* Steps to reproduce the issue

## Found a Problem in These Docs?

This documentation site itself is open on GitHub. If a page is wrong, outdated, or missing something, open an issue or pull request against it:

[SwagCore-docs on GitHub](https://github.com/swag617/SwagCore-docs)

## Useful Information for Any Support Request

* `/swagcore status` — shows version, loaded modules, online count, and Vault registration status
* `/swagcore reload` — use this before reporting config issues, to confirm your changes actually applied
* Check console output at startup for `"Registered as the Vault Economy provider."` and `"DashboardModule: registered at ..."` confirmations
* Config file is at `plugins/SwagCore/config.yml`
* SwagCore's data lives in SwagAPI's shared database, not a per-plugin file
