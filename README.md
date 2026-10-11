# Erila Network API

The HTTP API that game server plugins use to pick up store deliveries from
[erilanetwork.com](https://erilanetwork.com).

The website never connects to a game server. A plugin on each server polls the
website, runs whatever is queued for it on the console and reports back. If a
server is offline or restarting, nothing is lost: commands simply wait in the
queue until the plugin asks again. This is the same model Tebex and LeaderOS use.

## Contents

* [Base URL](#base-url)
* [Authentication](#authentication)
* [Conventions](#conventions)
* [Endpoints](#endpoints)
  * [`GET /ping/`](#get-ping)
  * [`POST /commands/`](#post-commands)
  * [`POST /commands/ack/`](#post-commandsack)
* [Delivery lifecycle](#delivery-lifecycle)
* [Limits](#limits)
* [Errors](#errors)
* [Writing a plugin](#writing-a-plugin)
* [Example client](#example-client)

## Base URL

```
https://erilanetwork.com/api/game/v1
```

Every path below is relative to this base. The API is versioned in the path;
breaking changes will ship under a new version.

## Authentication

Each game server has its own token. Staff create the server under
**Mağaza / Sunucular** in the admin panel, where the token is shown once. It
looks like `erila_` followed by 32 URL safe characters. If it is lost or
leaked, rotate it from the same page; the old token stops working right away.

Send it on every request:

```
Authorization: Bearer erila_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

A token belongs to exactly one server, so the API always knows who is calling.
You never pass a server id yourself.

Treat the token like a password. Anyone holding it can pull the commands queued
for that server. Keep it out of logs, crash reports and public repositories.

## Conventions

**Keep the trailing slash.** The site runs with trailing slashes on, so
`/commands` answers with a `308` redirect to `/commands/`. Plenty of HTTP
clients drop the POST body when they follow that redirect, and you will get an
empty response without any error. Always call the exact paths shown here.

**JSON in, JSON out.** Send `Content-Type: application/json`. A body that is
missing or isn't valid JSON is treated as an empty object, so a broken request
usually comes back as an empty result rather than a `400`.

**Ids are strings.** Command ids are 24 character hex strings. Pass them back
exactly as you got them.

## Endpoints

### `GET /ping/`

Checks the token without leasing anything. Call it once on startup so a bad
config fails loudly.

```bash
curl https://erilanetwork.com/api/game/v1/ping/ \
  -H "Authorization: Bearer $ERILA_TOKEN"
```

`200 OK`

```json
{ "ok": true, "server": "survival", "name": "Survival" }
```

| Field    | Type    | Description                                    |
| -------- | ------- | ---------------------------------------------- |
| `ok`     | boolean | Always `true` on success.                      |
| `server` | string  | Short name (slug) of the server this token is for. |
| `name`   | string  | Display name set in the panel.                 |

### `POST /commands/`

Sends the players online right now and gets back the commands to run. This is
also the server's heartbeat: the panel shows when the server was last seen and how
many players it has based on these calls. Poll every 5 to 10 seconds.

```bash
curl https://erilanetwork.com/api/game/v1/commands/ \
  -H "Authorization: Bearer $ERILA_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{ "online": ["Steve", "Alex"] }'
```

Request body

| Field    | Type     | Required | Description                                  |
| -------- | -------- | -------- | -------------------------------------------- |
| `online` | string[] | no       | Names of the players currently on this server. Send `[]` when nobody is online. |

Names have to match `^[A-Za-z0-9_]{1,16}$`. Anything else is silently dropped,
which matters on Bedrock: a gamertag with a space in it won't count as online,
so send the name the player registered with on the website. Matching is case
insensitive.

`200 OK`

```json
{
  "server": "survival",
  "commands": [
    {
      "id": "6720f0a1c3b5e2d4f6a7b8c9",
      "command": "lp user Steve parent add vip",
      "username": "Steve",
      "requireOnline": false
    },
    {
      "id": "6720f0a1c3b5e2d4f6a7b8ca",
      "command": "give Steve diamond 16",
      "username": "Steve",
      "requireOnline": true
    }
  ]
}
```

| Field                     | Type    | Description |
| ------------------------- | ------- | ----------- |
| `server`                  | string  | Slug of the calling server. |
| `commands`                | array   | Commands to run now, oldest first. Empty when there is nothing to do. |
| `commands[].id`           | string  | Pass this back to [`/commands/ack/`](#post-commandsack). |
| `commands[].command`      | string  | Ready for the console. No leading slash, no newlines. |
| `commands[].username`     | string  | The member the command was bought for. |
| `commands[].requireOnline`| boolean | `true` if the command needs the player in game (items, kits). These only come back while the player is in your `online` list. |

Every command returned here is **leased** to you for 60 seconds. Run it and ack
it within that window, otherwise it goes back to the queue and may be handed
out again.

### `POST /commands/ack/`

Reports the outcome of commands you were handed. Ack every one of them,
whether it worked or not.

```bash
curl https://erilanetwork.com/api/game/v1/commands/ack/ \
  -H "Authorization: Bearer $ERILA_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "results": [
      { "id": "6720f0a1c3b5e2d4f6a7b8c9", "success": true },
      { "id": "6720f0a1c3b5e2d4f6a7b8ca", "success": false, "error": "Unknown command" }
    ]
  }'
```

Request body

| Field               | Type    | Required | Description |
| ------------------- | ------- | -------- | ----------- |
| `results`           | array   | yes      | One entry per command. |
| `results[].id`      | string  | yes      | Id from `/commands/`. |
| `results[].success` | boolean | yes      | `true` if the console accepted the command. |
| `results[].error`   | string  | no       | Why it failed. Shown to staff, cut to 500 characters. |

`200 OK`

```json
{ "updated": 2 }
```

`updated` counts the commands whose state actually changed. It can be lower
than what you sent, and that is not an error. Entries are skipped when:

* the id is malformed,
* the command belongs to another server,
* the command is no longer leased (already acked, or already parked as failed).

Failed commands are not retried automatically. They appear under
**Loglar / Başarısız İşlemler** in the panel, where staff can put them back in
the queue after fixing the cause.

## Delivery lifecycle

1. A member buys a product. The order and its items land in the member's
   chest on the website.
2. The member presses **Teslim Al** in the chest. Every command of that product
   is rendered and queued for its target server as `pending`.
3. Your plugin polls `/commands/`. Matching commands move to `leased` for 60
   seconds and are returned.
4. Your plugin runs them and acks. Each one becomes `done` or `failed`.

```
pending ──poll──▶ leased ──ack ok──────▶ done
   ▲                │ │
   │   lease expires│ └─ack fail───────▶ failed ──staff retry──▶ pending
   └────────────────┘
          (after 5 leases with no ack the command is parked as failed)
```

A command with `requireOnline: false` (ranks, permissions, currency) is handed
out as soon as you poll. A command with `requireOnline: true` waits until the
player shows up in your `online` list.

Commands are leased atomically. Two plugin instances polling with the same
token at the same moment never get the same command, so a duplicated proxy
setup won't double deliver. It is still better to run one poller per token.

### Command templates

Staff write product commands in the panel with these placeholders. You receive
them already filled in; this is just so you know where the text comes from.

| Placeholder  | Value                                  |
| ------------ | -------------------------------------- |
| `{username}` | Website username of the member         |
| `{quantity}` | Purchased quantity                     |
| `{product}`  | Product name (quotes and backslashes replaced with spaces) |
| `{orderId}`  | Numeric order id                       |

## Limits

| What                              | Limit       |
| --------------------------------- | ----------- |
| Commands returned per poll        | 50          |
| Results processed per ack request | 200 (the rest are ignored) |
| Lease length                      | 60 seconds  |
| Leases before a command is parked | 5           |
| Online players stored per poll    | 500         |
| Error message length              | 500 characters |

If a poll comes back with 50 commands there may be more waiting. Ack them and
poll again right away instead of waiting for the next tick.

## Errors

| Status | Body                          | Meaning |
| ------ | ----------------------------- | ------- |
| `200`  | endpoint specific             | Success. |
| `308`  | none                          | You left out the trailing slash. Fix the path. |
| `401`  | `{ "error": "unauthorized" }` | Token missing, malformed or rotated. Don't retry; stop and log it. |
| `5xx`  | varies                        | Site side problem. Back off and retry; leased commands return to the queue on their own. |

## Writing a plugin

The plugin lives outside the website and can target any platform (Paper,
Velocity, Nukkit, PowerNukkitX, PocketMine, Endstone). Whatever it is, it
should:

* read the base URL and token from its config, never hardcode them,
* call `/ping/` on startup and disable itself with a clear message on `401`,
* poll `/commands/` on an async timer every 5 to 10 seconds with the online
  player names,
* run each command on the main thread through the console sender,
* ack every id with its real outcome, in one batch per poll,
* back off on network errors instead of hammering the site,
* never log the token.

## Example client

A minimal loop in TypeScript (Node 18+). Swap `runOnConsole` for your
platform's console dispatch.

```ts
const BASE = "https://erilanetwork.com/api/game/v1";
const TOKEN = process.env.ERILA_TOKEN!;

type Leased = { id: string; command: string; username: string; requireOnline: boolean };

async function call<T>(path: string, body?: unknown): Promise<T> {
  const res = await fetch(`${BASE}${path}`, {
    method: body === undefined ? "GET" : "POST",
    headers: {
      Authorization: `Bearer ${TOKEN}`,
      "Content-Type": "application/json",
    },
    body: body === undefined ? undefined : JSON.stringify(body),
  });
  if (!res.ok) throw new Error(`${path} answered ${res.status}`);
  return res.json() as Promise<T>;
}

async function tick(onlinePlayers: string[]) {
  const { commands } = await call<{ commands: Leased[] }>("/commands/", { online: onlinePlayers });
  if (!commands.length) return;

  const results = [];
  for (const c of commands) {
    try {
      await runOnConsole(c.command);
      results.push({ id: c.id, success: true });
    } catch (err) {
      results.push({ id: c.id, success: false, error: String(err) });
    }
  }
  await call("/commands/ack/", { results });
}
```

## Support

Questions or a broken delivery? Open a ticket on
[erilanetwork.com](https://erilanetwork.com).
