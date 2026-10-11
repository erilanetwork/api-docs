# Conventions

## Trailing slash

Always call the exact paths shown, trailing slash included. `/commands`
answers with a `308` redirect to `/commands/`, and many HTTP clients drop the
POST body when they follow it. You get an empty result with no error.

## JSON

Send `Content-Type: application/json`. Responses are always JSON.

A body that is missing or isn't valid JSON is treated as an empty object, so a
broken request usually comes back as an empty result rather than a `400`.

## Ids

Command ids are 24 character hex strings. Pass them back exactly as received.

[Back to contents](../README.md)
