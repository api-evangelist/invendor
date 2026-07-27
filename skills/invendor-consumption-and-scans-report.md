---
name: Pull consumption and scan activity
description: Read scan events, period consumption, and replenishment suggestions from the Invendor Reporting API.
api: openapi/invendor-reporting-openapi-original.json
operations:
  - GET /api/v1/Scans
  - GET /api/v1/Consumption
  - GET /api/v1/Replenishment
  - GET /api/v1/ItemQuantities/onhand
---

# Pull consumption and scan activity

Use the Invendor **Reporting API** (`https://reporting.invendor.com/api/v1`) — a read model over cabinet activity. Operations in this spec carry summaries but no `operationId`, so they are referenced by method + path.

## Auth
OAuth2 authorization-code, scope `io.reporting`; `Authorization: Bearer <token>`.

## Steps
1. Get scan events with `GET /api/v1/Scans`. If `dateFrom` is omitted it defaults to 3 months back; `dateTo` defaults to now.
2. Get period consumption with `GET /api/v1/Consumption`.
3. Get on-hand stock with `GET /api/v1/ItemQuantities/onhand`.
4. Get replenishment suggestions with `GET /api/v1/Replenishment` (and `GET /api/v1/Replenishment/order` for the current suggested order).

## Rules
- Scope is `io.reporting`, distinct from the Common API's `io.common` — request the right scope for the right host.
- `GET /api/v1/Scans/count` caps the period at 30 days (defaults to 7).
- These endpoints are read-only; write changes via the Common API.
