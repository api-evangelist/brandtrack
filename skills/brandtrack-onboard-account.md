---
name: Onboard a Brandtrack account (partner flow)
description: Register a new customer account and its first user via the Brandtrack partner API, then mint a user token.
api: openapi/brandtrack-openapi-original.yml
operations: [createAccount, createsANewTokenForRequestedUser]
---

# Onboard a Brandtrack account (partner flow)

Use this when you are a Brandtrack partner provisioning a new customer.

## Auth
- Partner endpoints authenticate with the `X-Partner-Key` header (not the customer `x-customer-api-key`).
- Base URL: `https://api.brandtrack.fm`.

## Steps
1. **Register the account + first user** — `POST /partner/accounts/register` (`createAccount`). Provide the account and initial user details in the JSON body. On success you receive the new account and user IDs.
2. **Mint a user token** — `POST /partner/users/{id}/token` (`createsANewTokenForRequestedUser`) with the new user's `id`. The returned token lets that user act; temporary user tokens are valid for 1 hour.

## Rules
- Errors return `{ message, status_code, errors? }` (not RFC 9457). Handle `422` by reading the per-field `errors` map; `402` means a plan limit was reached.
- No idempotency key exists — do not blind-retry `createAccount`; check for an existing account first.
