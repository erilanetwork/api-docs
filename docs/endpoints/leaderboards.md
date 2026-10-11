# Leaderboards

## `GET /leaderboards/`

Every board you can ask for.

```bash
curl https://erilanetwork.com/api/v1/leaderboards/ -H "Authorization: Bearer $ERILA_API_KEY"
```

`200 OK`

```json
{
  "leaderboards": [
    { "key": "network-level", "group": "network", "game": null, "unit": "level" },
    { "key": "bedwars-wins", "group": "network", "game": "bedwars", "unit": "count" },
    { "key": "skyblock-playtime", "group": "skyblock", "game": null, "unit": "seconds" },
    { "key": "credits-monthly", "group": "site", "game": null, "unit": "cents" }
  ]
}
```

The keys are:

| Key | Ranks by |
| --- | -------- |
| `network-level` | Network level |
| `{game}-wins`, `{game}-kills` | Wins or kills in one mode, for every `statsKey` listed in [Games](games.md) |
| `skyblock-island-level` | SkyBlock island level |
| `skyblock-playtime` | SkyBlock playtime |
| `credits-all-time`, `credits-monthly` | Credits loaded on the site, all time or this calendar month |

`unit` tells you how to read `value`: `count`, `level`, `seconds` or `cents`.

## `GET /leaderboards/{key}/`

One board, best first. All time; there are no weekly boards.

```bash
curl "https://erilanetwork.com/api/v1/leaderboards/bedwars-wins/?limit=3" -H "Authorization: Bearer $ERILA_API_KEY"
```

| Query   | Default | Description |
| ------- | ------- | ----------- |
| `limit` | 25      | Rows to return, 1 to 100. |

`200 OK`

```json
{
  "key": "bedwars-wins",
  "unit": "count",
  "rows": [
    { "rank": 1, "name": "Notch", "value": 21 },
    { "rank": 2, "name": "Herobrine", "value": 19 },
    { "rank": 3, "name": "Alex", "value": 14 }
  ]
}
```

| Field               | Type    | Description |
| ------------------- | ------- | ----------- |
| `rows[].rank`       | integer | Position, from 1. Ties are broken by name. |
| `rows[].name`       | string  | Player name, island name on `skyblock-island-level`, website username on the credit boards. |
| `rows[].value`      | integer | The ranked number, in `unit`. |
| `rows[].islandId`   | integer | Only on `skyblock-island-level`. |

Unknown keys answer `404`.

[Back to contents](../../README.md)
