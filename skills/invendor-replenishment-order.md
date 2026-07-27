---
name: Raise and fulfil a replenishment order
description: Create a replenishment order in the Invendor Common API and move it through confirm, ship, and receive.
api: openapi/invendor-common-openapi-original.json
operations:
  - CreateReplenishmentOrder
  - GetOrder
  - ConfirmOrder
  - ShipOrder
  - ReceiveOrder
  - CompleteOrder
---

# Raise and fulfil a replenishment order

Use the Invendor **Common API** to replenish a stocking location and track the order through its lifecycle.

## Auth
OAuth2 authorization-code, scope `io.common`; `Authorization: Bearer <token>`.

## Steps
1. Create the order with **CreateReplenishmentOrder** (`POST /v1/Orders/replenishment`). Capture the order id.
2. Inspect it with **GetOrder** (`GET /v1/Orders/{orderId}`).
3. Advance the state machine: **ConfirmOrder** (`POST /v1/Orders/{orderId}/confirm`) -> **ShipOrder** (`POST /v1/Orders/{orderId}/ship`) -> **ReceiveOrder** (`POST /v1/Orders/{orderId}/receive`) -> **CompleteOrder** (`POST /v1/Orders/{orderId}/complete`).

## Rules
- To decide *what* to reorder, read the Reporting API `GET /api/v1/Replenishment/order` (scope `io.reporting`) for the current suggested order.
- Errors follow `ProblemDetails` (`errors/invendor-problem-types.yml`); a `409 Conflict` typically means the order is not in a state that allows that transition.
- No idempotency key: capture the order id from step 1 and reuse it rather than re-posting.
