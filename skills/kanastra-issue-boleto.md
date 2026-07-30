---
name: Issue and track a boleto (bank slip)
description: Create bank slips in a wallet, fetch the PDF, and reconcile settlement via webhooks.
api: openapi/kanastra-banking-openapi.yml
operations: [walletList, bankSlipBoletoCreateBankSlips, bankSlipBoletoList, bankSlipBoletoGetPdfFile]
---

# Issue and track a boleto (bank slip)

## Steps
1. List wallets — `walletList` (`GET /api/v1/wallets`) to get a `wallet_uuid`.
2. Create slips in batch — `bankSlipBoletoCreateBankSlips`
   (`POST /api/v1/wallets/{wallet_uuid}/bank-slip/batch`).
3. Fetch the printable slip — `bankSlipBoletoGetPdfFile`
   (`GET /api/v1/wallets/{wallet_uuid}/bank-slip/{bank_slip_uuid}/pdf`).
4. List/query slips — `bankSlipBoletoList` (`GET /api/v1/wallets/{wallet_uuid}/bank-slip`).

## Idempotency
- `ourNumber` is the idempotence key **per wallet**. Set it to make issuance idempotent;
  omit it to have Kanastra generate the number plus its check digit (see
  conventions/kanastra-conventions.yml).

## Settlement
- Subscribe to webhooks (asyncapi/kanastra-banking-asyncapi.yml): `BANK_SLIP_OPEN`,
  `BANK_SLIP_SETTLED`, `BANK_SLIP_CANCELED`, `BANK_SLIP_REJECTED`, `BANK_SLIP_EXPIRED`,
  plus CNAB events for batch processing.
