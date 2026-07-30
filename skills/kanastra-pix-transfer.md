---
name: Register a PIX key and send a PIX transfer
description: Create/approve a PIX key on an account and send a PIX transfer by key.
api: openapi/kanastra-banking-openapi.yml
operations: [pixKeysCreateDocumentKey, pixKeysApproveKeyWithOtpToken, pixKeysListKeys, pixTransferTransferByPixKey, balanceRetrieve]
---

# Register a PIX key and send a PIX transfer

## Steps
1. Create a PIX key — `pixKeysCreateDocumentKey` (`POST /api/v1/pix/{account_uuid}/key`).
   (Variants create random, email, and phone keys on the same endpoint.)
2. Approve it with the OTP token — `pixKeysApproveKeyWithOtpToken`
   (`PATCH /api/v1/pix/{account_uuid}/key/{pix_key_uuid}`). Use `pixKeysResendOtpToken` if needed.
3. Verify — `pixKeysListKeys` (`GET /api/v1/pix/{account_uuid}/keys`).
4. Check funds — `balanceRetrieve` (`GET /api/v1/accounts/{account_uuid}/balance`).
5. Send — `pixTransferTransferByPixKey` (`POST /api/v1/pix/{account_uuid}/transfer`).
   (Variants support manual and scheduled transfers.)

## Rules
- Requires the `financial_account` scope on the Bearer token.
- Errors use a `{code, message}` envelope, e.g. `PIX_KEY_PIX_KEY_NOT_FOUND` (422); invalid
  identifiers return `{"message": "Invalid account UUID"}` (400). See errors/kanastra-problem-types.yml.
