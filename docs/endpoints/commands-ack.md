# `POST /commands/ack/`

Reports the outcome of leased commands. Ack every command you received,
whether it succeeded or not.

## Request

```bash
curl https://erilanetwork.com/api/game/v1/commands/ack/ \
  -H "Authorization: Bearer $ERILA_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "results": [
      { "id": "6720f0a1c3b5e2d4f6a7b8c9", "success": true },
      { "id": "6720f0a1c3b5e2d4f6a7b8ca", "success": false, "error": "Unknown command" }
    ]
  }'
```

| Field               | Type    | Required | Description |
| ------------------- | ------- | -------- | ----------- |
| `results`           | array   | yes      | One entry per command. |
| `results[].id`      | string  | yes      | Id from [`/commands/`](commands.md). |
| `results[].success` | boolean | yes      | `true` if the command ran. |
| `results[].error`   | string  | no       | Why it failed. Cut to 500 characters. |

## Response

`200 OK`

```json
{ "updated": 2 }
```

| Field     | Type    | Description |
| --------- | ------- | ----------- |
| `updated` | integer | Number of commands whose state changed. |

`updated` can be lower than the number of results sent. That is not an error.
An entry is skipped when:

* the id is malformed,
* the command belongs to another server,
* the command is not currently leased (already acked, for example).

Failed commands are not returned again automatically.

[Back to contents](../../README.md)
