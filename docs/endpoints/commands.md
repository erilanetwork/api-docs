# `POST /commands/`

Sends the players online right now and returns the commands this server should
run. Each call also counts as the server's heartbeat. Poll every 5 to 10
seconds.

## Request

```bash
curl https://erilanetwork.com/api/game/v1/commands/ \
  -H "Authorization: Bearer $ERILA_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{ "online": ["Steve", "Alex"] }'
```

| Field    | Type     | Required | Description |
| -------- | -------- | -------- | ----------- |
| `online` | string[] | no       | Names of the players currently on this server. Send `[]` when nobody is online. |

Names must match `^[A-Za-z0-9_]{1,16}$`. Other entries are silently dropped.
Matching is case insensitive.

## Response

`200 OK`

```json
{
  "server": "survival",
  "commands": [
    {
      "id": "6720f0a1c3b5e2d4f6a7b8c9",
      "command": "rank set Steve vip",
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

| Field                      | Type    | Description |
| -------------------------- | ------- | ----------- |
| `server`                   | string  | Short name of the calling server. |
| `commands`                 | array   | Commands to run now, oldest first. Empty when there is nothing to do. |
| `commands[].id`            | string  | Pass this back to [`/commands/ack/`](commands-ack.md). |
| `commands[].command`       | string  | Ready for the console. No leading slash, no newlines. |
| `commands[].username`      | string  | The player the command is for. |
| `commands[].requireOnline` | boolean | `true` if the command needs the player in game. These are only returned while the player is in your `online` list. |

## Leasing

Every returned command is leased to the caller for 60 seconds. Run it and ack
it within that window, otherwise it goes back to the queue and can be returned
again.

Leasing is atomic: two callers using the same token at the same moment never
receive the same command.

A response with 50 commands may mean more are waiting. Ack them and poll again
right away. See [Limits](../limits.md).

[Back to contents](../../README.md)
