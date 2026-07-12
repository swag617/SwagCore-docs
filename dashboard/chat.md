# Dashboard: Chat

The **Chat** tab shows a scrolling feed of recent in-game chat, and lets a signed-in staff member broadcast a message from the browser.

## Viewing chat

* `GET api/chat` returns the **50 most recent** messages from the `swagcore_chat_log` table, ordered newest-first, then the frontend reverses the list so the newest message renders at the bottom (like a normal chat window).
* Each entry shows a timestamp, the channel name, the player's display name (resolved via `Bukkit.getOfflinePlayer(uuid)`, falling back to the raw UUID if the name can't be resolved), and the message text.
* Message text is HTML-escaped client-side before insertion, so chat containing `<`, `>`, `&`, or quotes renders safely.
* The feed auto-scrolls to the bottom on load.

## Broadcasting a message

Typing into the input box and clicking **Send** (or pressing Enter) does:

```
POST api/chat
{ "message": "<your text>" }
```

The server broadcasts it to the whole server via `Bukkit.broadcastMessage`, prefixed with the signed-in SwagAPI panel username:

```
[Dashboard:<username>] <your message>
```

If no SwagAPI session username can be resolved, it falls back to the literal prefix `[Dashboard:dashboard]`. An empty or whitespace-only message is rejected with `400 Empty message`. After a successful send, the chat feed reloads to show the new broadcast.

## Related Pages

* [Dashboard Overview & Access](overview.md) — how the signed-in username is resolved
* [Punishments](punishments.md) — a related staff-facing tab
