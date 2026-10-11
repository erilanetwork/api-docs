# Rate limits

Each key may make **60 requests per minute**. The window is fixed: it resets
at the start of every minute.

Going over answers `429`:

```
HTTP/1.1 429 Too Many Requests
Retry-After: 17

{ "error": "rate_limited" }
```

`Retry-After` is the number of seconds until the window resets. Wait that
long before trying again.

Other limits:

| What                          | Limit |
| ----------------------------- | ----- |
| Rows per leaderboard request  | 100 (`limit`, default 25) |
| Islands per request           | 100 (`limit`, default 25) |
| News per page                 | 20    |

[Back to contents](../README.md)
