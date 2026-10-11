# Authentication

Every request needs a server token in the `Authorization` header:

```
Authorization: Bearer erila_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Tokens start with `erila_` followed by 32 URL safe characters. Each token
belongs to exactly one game server, so the API always knows who is calling and
you never pass a server id yourself.

Tokens are issued by Erila Network staff. A rotated token stops working right
away.

A missing, malformed or rotated token gets `401`. See [Errors](errors.md).

Treat the token like a password. Anyone holding it can pull the commands queued
for that server, so keep it out of logs and public repositories.

[Back to contents](../README.md)
