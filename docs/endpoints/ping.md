# `GET /ping/`

Checks the token without leasing any commands. Useful on startup so a bad
token fails loudly.

## Request

```bash
curl https://erilanetwork.com/api/game/v1/ping/ \
  -H "Authorization: Bearer $ERILA_TOKEN"
```

No body.

## Response

`200 OK`

```json
{ "ok": true, "server": "survival", "name": "Survival" }
```

| Field    | Type    | Description                                     |
| -------- | ------- | ----------------------------------------------- |
| `ok`     | boolean | Always `true` on success.                       |
| `server` | string  | Short name of the server this token belongs to. |
| `name`   | string  | Display name of the server.                     |

[Back to contents](../../README.md)
