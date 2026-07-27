---
name: Catalog an item and attach a barcode
description: Create an inventory item in the Invendor Common API and register the barcode(s) a smart cabinet or scanner will use to identify it.
api: openapi/invendor-common-openapi-original.json
operations:
  - CreateItem
  - GetItemDetails
  - CreateItemBarcodes
  - GetItemBarcodes
  - GetItems
---

# Catalog an item and attach a barcode

Use the Invendor **Common API** (`https://api.invendor.com/v1`) to add a stock item and the barcode(s) that identify it at a scan/weight cabinet.

## Auth
OAuth2 authorization-code, scope `io.common`. Send `Authorization: Bearer <token>` on every request. Tokens come from `https://identity.scanbro.com/connect/token`.

## Steps
1. (Optional) Check whether the item already exists with **GetItems** (`GET /v1/Items`, paginate with `Page`/`PageSize`).
2. Create the item with **CreateItem** (`POST /v1/Items`). Capture the returned item id.
3. Register its barcode(s) with **CreateItemBarcodes** (`POST /v1/Items/{itemId}/barcodes`).
4. Verify with **GetItemDetails** (`GET /v1/Items/{itemId}`) and **GetItemBarcodes** (`GET /v1/Items/{itemId}/barcodes`).

## Rules
- Errors return the RFC 7807 `ProblemDetails` shape as `application/json` (see `errors/invendor-problem-types.yml`).
- No idempotency key is supported; guard re-runs by checking existence first (step 1).
- For large loads prefer the bulk endpoint `POST /v1/Items/bulk`.
