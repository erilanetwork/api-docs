# Islands

## `GET /islands/`

SkyBlock islands by level, highest first.

```bash
curl "https://erilanetwork.com/api/v1/islands/?limit=2" -H "Authorization: Bearer $ERILA_API_KEY"
```

| Query   | Default | Description |
| ------- | ------- | ----------- |
| `limit` | 25      | Islands to return, 1 to 100. |

`200 OK`

```json
{
  "islands": [
    { "id": 2, "name": "Mira'nın Bahçesi", "owner": "Mira", "partners": [], "level": 22, "createdAt": "2026-09-11T15:45:49.165Z" },
    { "id": 1, "name": "Gökyüzü Kalesi", "owner": "Steve", "partners": ["Alex", "Kai"], "level": 14, "createdAt": "2026-07-13T15:45:49.165Z" }
  ]
}
```

| Field       | Type     | Description |
| ----------- | -------- | ----------- |
| `id`        | integer  | Use with `GET /islands/{id}/`. |
| `name`      | string   | Island name. |
| `owner`     | string   | Owner's player name. |
| `partners`  | string[] | Partners' player names. |
| `level`     | integer  | Island level. |
| `createdAt` | string   | When the island was founded. |

## `GET /islands/{id}/`

One island, same fields. Unknown ids answer `404`.

[Back to contents](../../README.md)
