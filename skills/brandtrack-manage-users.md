---
name: Invite and manage Brandtrack users
description: Invite a user to a Brandtrack account, issue them a temporary token, and manage suspension state.
api: openapi/brandtrack-openapi-original.yml
operations: [invitesAUserToThePlatform, listUsers, createTemporaryAuthToken, handleUserSuspensionStatus]
---

# Invite and manage Brandtrack users

## Auth
- `x-customer-api-key` header. Base URL `https://api.brandtrack.fm`.

## Steps
1. **Invite a user** — `POST /v2/users` (`invitesAUserToThePlatform`) with the user's details and role.
2. **Confirm** — `GET /v2/users` (`listUsers`) to find the new user's `id` (supports `page`/`per_page`/`order_by`/`direction`).
3. **Issue a temporary token** — `POST /v2/users/{id}/token` (`createTemporaryAuthToken`); the token is valid for 1 hour, useful for scoped/embedded sessions.
4. **Suspend / unsuspend** — `POST /v2/users/{id}/suspension` (`handleUserSuspensionStatus`). A `409` means the user is already in the requested state — fetch current state first.

## Rules
- Errors return `{ message, status_code, errors? }`; `403` = unauthorized action or wrong ID.
- No idempotency key — check `listUsers` before re-inviting to avoid duplicates.
