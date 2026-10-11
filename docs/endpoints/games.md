# Games

## `GET /games/`

Every game mode shown on the site, in display order.

```bash
curl https://erilanetwork.com/api/v1/games/ -H "Authorization: Bearer $ERILA_API_KEY"
```

`200 OK`

```json
{
  "games": [
    {
      "slug": "bedwars",
      "name": "BedWars",
      "description": "Yatağını koru, rakiplerinkini yık.",
      "image": "https://erilanetwork.com/assets/games/bedwars.png",
      "statsKey": "bedwars",
      "playing": 40
    },
    {
      "slug": "skyblock",
      "name": "SkyBlock",
      "description": "Adanı kur, büyüt, ortaklarınla zirveye çık.",
      "image": null,
      "statsKey": null,
      "playing": 61
    }
  ]
}
```

| Field         | Type          | Description |
| ------------- | ------------- | ----------- |
| `slug`        | string        | Use with `GET /games/{slug}/`. |
| `name`        | string        | Display name. |
| `description` | string        | Short description. |
| `image`       | string, null  | Absolute image URL. |
| `statsKey`    | string, null  | Key the mode's stats and leaderboards use (`bedwars`, `skywars`, `thebridge`, `murdermystery`, `woolbattle`, `duels`, `zombies`, `amongus`, `football`). `null` for SkyBlock. |
| `playing`     | integer, null | Players in this mode right now. `null` when unknown. |

## `GET /games/{slug}/`

One game mode, same fields as above. Unknown or hidden slugs answer `404`.

```bash
curl https://erilanetwork.com/api/v1/games/bedwars/ -H "Authorization: Bearer $ERILA_API_KEY"
```

[Back to contents](../../README.md)
