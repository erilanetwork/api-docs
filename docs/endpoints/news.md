# News

Answers `404` while the blog is switched off on the site.

## `GET /news/`

News posts, newest first, 20 per page.

```bash
curl "https://erilanetwork.com/api/v1/news/?page=1" -H "Authorization: Bearer $ERILA_API_KEY"
```

| Query  | Default | Description |
| ------ | ------- | ----------- |
| `page` | 1       | Page number. |

`200 OK`

```json
{
  "page": 1,
  "perPage": 20,
  "total": 2,
  "news": [
    {
      "slug": "skyblock-sunucumuz-v2-guncellemesiyle-aciliyor",
      "title": "SKYBLOCK SUNUCUMUZ V2 GÜNCELLEMESİYLE AÇILIYOR!",
      "category": "SkyBlock",
      "image": "https://erilanetwork.com/assets/blog/v2.png",
      "excerpt": "Uzun süredir beklenen güncelleme geliyor.",
      "author": "Erila",
      "publishedAt": "2026-03-01T18:00:00.000Z"
    }
  ]
}
```

A page past the end returns an empty `news` list.

## `GET /news/{slug}/`

One post with its `body` (sanitised HTML) on top of the fields above.
Unknown slugs answer `404`.

[Back to contents](../../README.md)
