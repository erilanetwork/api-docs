# Store

Read only views of the store catalogue. Both answer `404` while the store is
switched off on the site. Buying is not part of the API.

## `GET /store/categories/`

```bash
curl https://erilanetwork.com/api/v1/store/categories/ -H "Authorization: Bearer $ERILA_API_KEY"
```

`200 OK`

```json
{
  "categories": [
    { "slug": "skyblock-2", "name": "SkyBlock", "image": "https://erilanetwork.com/assets/store/skyblock.png", "parent": null }
  ]
}
```

| Field    | Type          | Description |
| -------- | ------------- | ----------- |
| `slug`   | string        | Use as `category` on `GET /store/products/`. |
| `name`   | string        | Display name. |
| `image`  | string, null  | Absolute image URL. |
| `parent` | string, null  | Parent category slug for sub categories. |

## `GET /store/products/`

Products on sale.

```bash
curl "https://erilanetwork.com/api/v1/store/products/?category=skyblock-2" -H "Authorization: Bearer $ERILA_API_KEY"
```

| Query      | Default | Description |
| ---------- | ------- | ----------- |
| `category` | all     | Only products in this category slug. |

`200 OK`

```json
{
  "products": [
    {
      "id": 1,
      "name": "VIP",
      "category": "vip-kasa-urunleri-1",
      "categoryName": "VIP/Kasa Ürünleri",
      "description": "<p>/fly komudunu kullanabilme.</p>",
      "image": "https://erilanetwork.com/assets/store/vip.png",
      "priceCents": 2500,
      "salePriceCents": 2000,
      "available": true
    }
  ]
}
```

| Field            | Type          | Description |
| ---------------- | ------------- | ----------- |
| `id`             | integer       | Product id. Its page is `https://erilanetwork.com/store/products/{id}/`. |
| `priceCents`     | integer       | List price in cents of the site's credits. |
| `salePriceCents` | integer       | What it costs right now, after any running discount. |
| `available`      | boolean       | `false` when sold out. |

[Back to contents](../../README.md)
