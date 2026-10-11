# Members

## `GET /members/{username}/`

A site account by its website username, matched case insensitively.

```bash
curl https://erilanetwork.com/api/v1/members/SteveSite/ -H "Authorization: Bearer $ERILA_API_KEY"
```

`200 OK`

```json
{ "username": "SteveSite", "role": "Oyuncu", "registeredAt": "2024-04-05T15:33:00.000Z", "gameName": "Steve" }
```

| Field          | Type          | Description |
| -------------- | ------------- | ----------- |
| `username`     | string        | Website username. |
| `role`         | string        | Site role. |
| `registeredAt` | string, null  | When the account was created. |
| `gameName`     | string, null  | Linked game account, usable with [Players](players.md). |

E-mail, balance, Discord ids and anything else private are never returned.
Unknown members answer `404`.

[Back to contents](../../README.md)
