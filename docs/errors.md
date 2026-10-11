# Errors

| Status | Body                          | Meaning |
| ------ | ----------------------------- | ------- |
| `200`  | endpoint specific             | Success. |
| `308`  | none                          | Trailing slash missing. Fix the path, see [Conventions](conventions.md). |
| `401`  | `{ "error": "unauthorized" }` | Token missing, malformed or rotated. Don't retry. |
| `5xx`  | varies                        | Temporary problem on our side. Back off and retry. Leased commands return to the queue on their own. |

[Back to contents](../README.md)
