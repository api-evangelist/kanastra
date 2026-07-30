---
name: Authenticate with Kanastra Banking
description: Register an ES512 key and obtain a Bearer JWT access token to call the Kanastra Banking API.
api: openapi/kanastra-banking-openapi.yml
operations: [authenticationCreateJwks, authenticationGenerateAuthorizationToken]
---

# Authenticate with Kanastra Banking

Kanastra uses a private_key_jwt (client-assertion) flow. Sandbox base URL is
`https://banking-sandbox.kanastra.com.br`; production is `https://banking.kanastra.com.br`.

## Steps
1. Generate an ES512 (secp521r1) key pair. Keep the private key secret.
2. Register the public key for your `clientId` — `authenticationCreateJwks`
   (`POST /api/v1/auth/jwks`) with body `{ "clientId": "...", "publicKey": "..." }`.
3. Build and sign a JWT client assertion with your ES512 private key.
4. Exchange it — `authenticationGenerateAuthorizationToken`
   (`POST /api/v1/auth/token`) with `{ "clientId": "...", "clientAssertion": "<jwt>" }`.
   The response returns `accessToken` (Bearer), `expiresIn` (~35999s) and a space-delimited
   `scope` (e.g. `financial_account bank_slip bank_slip:create commercial_paper webhook`).
5. Send `Authorization: Bearer <accessToken>` on all subsequent calls.

## Rules
- `clientId` is issued by Kanastra during onboarding.
- Account-creation endpoints also require an HMAC-SHA256 Transaction Hash Key header.
- Every response carries an `x-request-id` header — log it for support.
