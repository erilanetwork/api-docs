# Erila Network API

Reference for the public Erila Network API.

## Contents

* [Authentication](docs/authentication.md)
* [Conventions](docs/conventions.md)
* Endpoints
  * [`GET /ping/`](docs/endpoints/ping.md)
  * [`POST /commands/`](docs/endpoints/commands.md)
  * [`POST /commands/ack/`](docs/endpoints/commands-ack.md)
* [Limits](docs/limits.md)
* [Errors](docs/errors.md)

## Base URL

```
https://erilanetwork.com/api/game/v1
```

Every path in this reference is relative to the base URL. The version is part
of the path; breaking changes ship under a new version.
