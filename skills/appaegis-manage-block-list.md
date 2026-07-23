---
name: Manage the Mammoth Cyber block list
description: Authenticate to the Mammoth Cyber (Appaegis) management API and upload or replace the organization blocked-site list.
api: https://api.mammothcyber.net
operations: [authenticate, update_blocked_sites]
source: https://github.com/appaegis/api-script-samples/blob/main/block-list-v2.py
---

# Manage the Mammoth Cyber block list

Use this to push a set of blocked domains to a Mammoth Cyber (Appaegis) tenant.

## Prerequisites
- An API key/secret pair created in the management portal: Setting -> Admins & API Keys -> API Keys (the secret is shown once; download the dotenv file).
- Base URL: `https://api.mammothcyber.net`.

## Steps
1. **Authenticate.** `POST /api/v2/authentication` with body `{"apiKey": "...", "apiSecret": "..."}` and `content-type: application/json`. Read the returned `Authorization` field — that is your short-lived `idToken`.
2. **Upload the block list.** `PUT /api/v2/blocked-sites` with header `idToken: <idToken>` and a JSON body containing the domains. For a large list, extract the longest matching domain per entry (as the sample does) before sending.
3. **Handle errors.** Treat HTTP status >= 400 or an `error` key in the JSON body as failure (per lib/common.py). A 400 on authentication means the credential body was malformed or empty.

## Notes
- idTokens are short-lived; re-authenticate rather than caching indefinitely.
- No idempotency key is supported; the PUT replaces the current list.
