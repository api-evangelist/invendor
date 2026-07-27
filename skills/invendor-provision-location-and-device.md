---
name: Provision a location and bind a cabinet device
description: Create a stocking location in the Invendor Common API and attach a smart cabinet / scanner device to it.
api: openapi/invendor-common-openapi-original.json
operations:
  - Create location
  - Add location device
  - LocationDevicesList
  - Devices list
---

# Provision a location and bind a cabinet device

Use the Invendor **Common API** to stand up a customer stocking location and register the hardware that services it.

## Auth
OAuth2 authorization-code, scope `io.common`; `Authorization: Bearer <token>`.

## Steps
1. Create the location with **Create location** (`POST /v1/Locations`). Capture the location id.
2. List available/registered hardware with **Devices list** (`GET /v1/Devices`).
3. Bind a device to the location with **Add location device** (`POST /v1/Locations/{locationId}/devices`).
4. Confirm the binding with **LocationDevicesList** (`GET /v1/Locations/{locationId}/devices`).

## Rules
- Send commands to a bound cabinet with `POST /v1/Devices/{deviceId}/command`.
- Errors follow `ProblemDetails` (`errors/invendor-problem-types.yml`); a 403 means the token's account lacks access to that location.
