---
title: Armadillo API overview
date: 2025-12-02
bread: false
toc: true
moredocs: false
totop: false
---
API ver. `1.1`
# Summary

The Armadillo API uses HTTP for calls, REST for endpoints, and JSON for request and response bodies. Methods used: `POST`, `GET`, `PATCH`, `DELETE`. Authentication is based on OAuth 2.0 tokens.

The Armadillo API allows registered users to perform typical ride hailing actions via their client (mobile app): to call a cab as a passenger, to provide a cab as a driver.

# Access roles and scopes

Functionality is distributed to users through system roles. Recognized system roles and scopes (available functionality) are as follows:

| Role      | Scopes                                                                                                   |
| --------- | -------------------------------------------------------------------------------------------------------- |
| Passenger | Get Fare estimates; submit, rate, cancel Trips.                                                   |
| Driver    | Accept/decline, close Trips.                                                                           |
| Admin     | Issue and manage coupons; process trip ratings and user complaints; issue refunds; etc. |

> [!Note] A user can have one system role at a time. 

# Data types and formats

The Armadillo API uses the following data types and formats:

> [!NOTE] Field names are case-sensitive.

| Data type | Description                                                                                                                                                     |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| string    | String of UTF-8 characters. IDs are always unique and encrypted. Timestamps are always UTC ISO 8601 (e.g., `2074-07-15T09:17:51Z`), unless specified otherwise. |
| boolean   | True/false variable. Possible values: `true`, `false`.                                                                                                          |
| float     | Decimal numbers. Coordinates are always stored until 6 digits after decimal point. Distance is always in miles.                                                 |
| integer   | Whole numbers.                                                                                                                                                  |
| object    | Collection of "key-value" pairs.                                                                                                                                |
| array     | Ordered list of values or objects.                                                                                                                              |

# Versioning

The Armadillo API uses semantic versioning: "X.X.X", where:

- **X**.x.x - major release version (breaking changes).
- x.**X**.x - minor release version (non-breaking changes).
- x.x.**X** - patch version (only fixes).

# URLs 

- Sandbox: `https://sb-env.armadillo.pub/api`
- Production: `https://armadillo.pub/api`
- Status page: `https://status.armadillo.pub`

# Rate limits

The Armadillo API can handle lots of requests. However, to ensure stable service provision and prevent abuse, rate limits may apply based on the traffic patterns, endpoint, and User account type. If a request hits the limit, API returns `429 "too many requests"`. In this case, clients should pause requests for some time as repeated retries without delay may extend the request blocking.

# Errors

Armadillo API errors follow the below model:

```json
{
	"message": "ERROR_MESSAGE",
	"details": "ERROR_DESCRIPTION"
}
```

Possible endpoint-specific errors are listed in the description of the corresponding endpoint.

Some of the common errors are:

| Status | Message                 | Details                                                                                                                                                              |
| ------ | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 400    | "bad request"           | Malformed JSON or validation errors.                                                                                                                                 |
| 401    | "unauthorized"          | Missing or invalid auth token.                                                                                                                                       |
| 403    | "forbidden"             | User not allowed to execute the action.                                                                                                                              |
| 404    | "not found"             | Target entities/records do not exist.                                                                                                                                |
| 409    | "target conflict"       | Idempotency conflict.                                                                                                                                                |
| 422    | "unprocessable entity"  | Request is received but is logically invalid (e.g. pickup and dropoff coordinates valid, but point to the same location).                                            |
| 429    | "too many requests"     | Rate limit exceeded.                                                                                                                                                 |
| 500    | "internal server error" | Internal error of unknown nature (uncaught exceptions, DB connection failures, etc.).                                                                                |
| 503    | "service unavailable"   | One or more services involved in the request are not available (distance calculation, fare estimation, etc.). Refer to `https://status.armadillo.pub` and try again. |
