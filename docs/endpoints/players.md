# Players

## `GET /players/{name}/`

A player's public profile: network level, per mode stats and SkyBlock. The
name is the in game name and is matched case insensitively.

```bash
curl https://erilanetwork.com/api/v1/players/Steve/ -H "Authorization: Bearer $ERILA_API_KEY"
```

`200 OK`

```json
{
  "name": "Steve",
  "badges": ["verified", "discord", "level"],
  "discordLinked": true,
  "member": { "username": "SteveSite", "role": "Oyuncu" },
  "network": {
    "firstJoin": "2024-01-15T15:45:48.423Z",
    "lastJoin": "2026-10-11T14:45:48.423Z",
    "level": 28,
    "xp": 111,
    "levelRank": { "key": "copper", "name": "Bakır", "level": 20 },
    "modes": [
      { "game": "bedwars", "played": 10, "wins": 7, "kills": 13, "deaths": 5 }
    ]
  },
  "skyblock": {
    "firstJoin": "2024-01-15T15:45:48.423Z",
    "lastJoin": "2026-10-11T15:45:48.423Z",
    "playTimeSeconds": 40000,
    "kills": 3,
    "deaths": 1,
    "island": {
      "id": 1,
      "name": "Gökyüzü Kalesi",
      "owner": "Steve",
      "partners": ["Alex", "Kai"],
      "level": 14,
      "createdAt": "2026-07-13T15:45:49.165Z"
    }
  }
}
```

| Field           | Type          | Description |
| --------------- | ------------- | ----------- |
| `name`          | string        | Name as the game spells it. |
| `badges`        | string[]      | Any of `verified` (linked to a site account), `discord`, `supporter` (has bought from the store), `staff`, `level` (reached a level rank). |
| `discordLinked` | boolean       | Linked a Discord account in game. |
| `member`        | object, null  | The site account linked to this player, see [Members](members.md). |
| `network`       | object, null  | `null` if the player never joined the minigame network. |
| `network.levelRank` | object, null | Highest level rank reached: Kömür 10, Bakır 20, Demir 50, Altın 70, Elmas 100, Zümrüt 150, Netherite 200. |
| `network.modes` | array         | One entry per mode played. K/D and win rate are left to you. |
| `skyblock`      | object, null  | `null` if the player never played SkyBlock. |
| `skyblock.island` | object, null | The island they own or are a partner on, see [Islands](islands.md). |

Unknown players answer `404`. Punishments are never part of a profile.

[Back to contents](../../README.md)
