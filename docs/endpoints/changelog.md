# Changelog

## `GET /changelog/`

Release notes, newest first. Answers `404` while the blog is switched off.

```bash
curl https://erilanetwork.com/api/v1/changelog/ -H "Authorization: Bearer $ERILA_API_KEY"
```

`200 OK`

```json
{
  "changelog": [
    { "id": 4, "version": "1.4.0", "title": "Ada pazarı", "body": "<p>Adalar artık satılabiliyor.</p>", "publishedAt": "2026-10-01T12:00:00.000Z" }
  ]
}
```

| Field         | Type    | Description |
| ------------- | ------- | ----------- |
| `id`          | integer | Entry id. |
| `version`     | string  | Version label, free text. |
| `title`       | string  | Title. |
| `body`        | string  | Sanitised HTML. |
| `publishedAt` | string  | Publish date. |

[Back to contents](../../README.md)
