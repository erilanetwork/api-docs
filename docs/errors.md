# Errors

Errors are JSON with a single `error` code:

```json
{ "error": "not_found" }
```

| Status | `error`                 | Meaning |
| ------ | ----------------------- | ------- |
| `401`  | `unauthorized`          | Key missing, malformed, rotated, deleted, or the account is banned. Don't retry. |
| `404`  | `not_found`             | Unknown player, member, island, board, news post or path. Also returned when the part of the site an endpoint belongs to (news, store) is switched off. |
| `429`  | `rate_limited`          | Over 60 requests this minute. Wait for `Retry-After` seconds. See [Rate limits](rate-limits.md). |
| `503`  | `game_data_unavailable` | Game data can't be reached right now. Site data (news, store, members) still works. Retry later. |
| `5xx`  | varies                  | Something went wrong on our side. Back off and retry. |

A `308` means the trailing slash is missing, see [Conventions](conventions.md).

[Back to contents](../README.md)
