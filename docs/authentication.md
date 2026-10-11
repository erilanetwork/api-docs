# Authentication

Every request needs your API key in the `Authorization` header:

```
Authorization: Bearer erk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Keys start with `erk_` followed by 32 URL safe characters.

## Getting a key

Sign in on [erilanetwork.com](https://erilanetwork.com), open your profile and
go to the **API** tab. Each account has one key.

* **Create** shows the key once. Copy it then; it can't be shown again.
* **Rotate** replaces it with a new one. The old key stops working right away.
* **Delete** removes it.

The tab also shows when the key was created and when it was last used.

## Keeping it safe

* Keep the key on a server. There are no CORS headers, so browser code can't
  call the API anyway, and a key shipped to a browser is a key anyone can copy.
* Don't commit it to a repository or paste it in public channels.
* If it leaks, rotate it.

A missing, malformed, rotated or deleted key gets `401`. So does the key of a
banned account. See [Errors](errors.md).

[Back to contents](../README.md)
