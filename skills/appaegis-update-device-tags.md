---
name: Update Mammoth Cyber registered-device tags
description: Authenticate to the Mammoth Cyber (Appaegis) management API and update the tag on registered devices, optionally from a CSV.
api: https://api.mammothcyber.net
operations: [authenticate, update_registered_device_tag]
source: https://github.com/appaegis/api-script-samples/blob/main/device-tag-update.py
---

# Update Mammoth Cyber registered-device tags

Use this to bulk-update tags on devices registered in a Mammoth Cyber (Appaegis) tenant.

## Prerequisites
- API key/secret pair from the management portal (Setting -> Admins & API Keys).
- Base URL: `https://api.mammothcyber.net`.
- A device id (and desired tag) per update; the sample reads these from a CSV.

## Steps
1. **Authenticate.** `POST /api/v2/authentication` with `{"apiKey": "...", "apiSecret": "..."}`; take the `Authorization` field as your `idToken`.
2. **Update each device.** For each device, `PUT /api/v2/registered-device/{id}` with header `idToken: <idToken>` and a JSON body carrying the new tag. URL-encode the `{id}` path segment.
3. **Dry run first.** The sample supports `--dry-run=true`; validate the CSV mapping before sending live updates.
4. **Handle errors.** HTTP >= 400 or an `error` key in the response body indicates failure.

## Notes
- Iterate device-by-device; there is no documented bulk endpoint.
- No idempotency key; a repeated PUT simply re-sets the same tag.
