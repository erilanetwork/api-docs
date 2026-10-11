# Servers

## `GET /servers/`

The servers behind the network and whether they are up.

```bash
curl https://erilanetwork.com/api/v1/servers/ -H "Authorization: Bearer $ERILA_API_KEY"
```

`200 OK`

```json
{
  "servers": [
    { "slug": "lobby", "name": "Lobi", "online": true, "players": 21 },
    { "slug": "skyblock", "name": "SkyBlock", "online": true, "players": 61 }
  ]
}
```

| Field                 | Type    | Description |
| --------------------- | ------- | ----------- |
| `servers[].slug`      | string  | Short name. |
| `servers[].name`      | string  | Display name. |
| `servers[].online`    | boolean | Seen in the last minute. |
| `servers[].players`   | integer | Players on that server. For the lobby this is only who stands in the lobby; players inside a game count under [Status](status.md) `modes`. |

[Back to contents](../../README.md)
