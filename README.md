# Erila Network API

A read only JSON API for building bots, widgets and stat sites on top of
Erila Network: server status, game modes, player profiles, leaderboards,
SkyBlock islands, news and the store catalogue.

## Base URL

```
https://erilanetwork.com/api/v1
```

Every path in this reference is relative to the base URL. The version is part
of the path; breaking changes ship under a new version.

## Quick start

1. Sign in on [erilanetwork.com](https://erilanetwork.com), open your profile
   and create a key on the **API** tab.
2. Send it with every request:

```bash
curl https://erilanetwork.com/api/v1/status/ \
  -H "Authorization: Bearer $ERILA_API_KEY"
```

```json
{ "totalPlayers": 128, "modes": { "bedwars": 40, "skywars": 12 }, "skyblock": 61, "maintenance": false }
```

## Contents

* [Authentication](docs/authentication.md)
* [Conventions](docs/conventions.md)
* [Rate limits](docs/rate-limits.md)
* [Errors](docs/errors.md)
* Endpoints
  * [Status](docs/endpoints/status.md): `GET /status/`
  * [Servers](docs/endpoints/servers.md): `GET /servers/`
  * [Games](docs/endpoints/games.md): `GET /games/`, `GET /games/{slug}/`
  * [Players](docs/endpoints/players.md): `GET /players/{name}/`
  * [Members](docs/endpoints/members.md): `GET /members/{username}/`
  * [Leaderboards](docs/endpoints/leaderboards.md): `GET /leaderboards/`, `GET /leaderboards/{key}/`
  * [Islands](docs/endpoints/islands.md): `GET /islands/`, `GET /islands/{id}/`
  * [News](docs/endpoints/news.md): `GET /news/`, `GET /news/{slug}/`
  * [Changelog](docs/endpoints/changelog.md): `GET /changelog/`
  * [Store](docs/endpoints/store.md): `GET /store/categories/`, `GET /store/products/`

## Fair use

Cache what you can, poll status no more than once every 30 seconds, and keep
your key on a server. Keys that are abused get revoked.
