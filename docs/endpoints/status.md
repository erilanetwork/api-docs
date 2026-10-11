# Status

## `GET /status/`

How many people are playing right now.

```bash
curl https://erilanetwork.com/api/v1/status/ -H "Authorization: Bearer $ERILA_API_KEY"
```

`200 OK`

```json
{
  "totalPlayers": 128,
  "modes": { "bedwars": 40, "skywars": 12, "thebridge": 6 },
  "skyblock": 61,
  "maintenance": false
}
```

| Field          | Type           | Description |
| -------------- | -------------- | ----------- |
| `totalPlayers` | integer        | Everyone online across the network: lobby, game modes and SkyBlock. |
| `modes`        | object         | Players per game mode, keyed by the mode's `statsKey` (see [Games](games.md)). Modes nobody plays may be missing. |
| `skyblock`     | integer, null  | Players on SkyBlock, `null` when it couldn't be asked. |
| `maintenance`  | boolean        | `true` while the site is in maintenance mode. |

Works even when game data is unavailable; counts then fall back to what was
last reported.

[Back to contents](../../README.md)
