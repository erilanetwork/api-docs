# Conventions

## Trailing slash

Call the paths exactly as shown, trailing slash included. A path without it
answers with a `308` redirect, and many HTTP clients drop the
`Authorization` header when they follow one.

## Requests

Every endpoint is `GET`. Path parameters are case insensitive where they are
names (players, members). Query parameters are optional.

## Responses

* Always JSON, UTF 8.
* Dates are ISO 8601 strings in UTC, for example `"2026-10-11T12:00:00.000Z"`.
* Durations are whole seconds (`playTimeSeconds`).
* Money is whole cents (`priceCents`), in the site's credit currency.
* Image fields are absolute URLs, or `null`.
* Missing data is `null`, never an absent field.
* Text fields named `description` or `body` contain sanitised HTML.

## Caching

Responses carry `Cache-Control: private, max-age=30`. Game data (players,
leaderboards, islands) is refreshed on our side about once a minute, so
polling faster than that returns the same numbers.

[Back to contents](../README.md)
