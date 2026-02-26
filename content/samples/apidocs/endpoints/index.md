---
title: Endpoints
date: 2025-12-18
bread: false
toc2: true
totop: true
---
# Get Fare estimate

**`1.1`**

`POST` /fares

Estimate the cost of a trip before submitting it.

### Request

**Authorization**

Bearer token with any relevant scope.

\
**Path params**

none

\
**Query params**

none

\
**Headers**

- Idempotency-Key (string) - Ensures the request is processed only once. required - **yes**

\
**Body**

- `passenger_id` (string) - Passenger identifier. required - **yes**
- `pickup` (object) - Pickup point coordinates. required - **yes**

	- `lat` (number|float) - Pickup point latitude. required - **yes**
	- `long` (number|float) - Pickup point longitude. required - **yes**
- `dropoff` (object) - Dropoff point coordinates. required - **yes**

	- `lat` (number|float) - Dropoff point latitude. required - **yes**
	- `long` (number|float) - Dropoff point longitude. required - **yes**
- `coupon_id` (string) - ID of the Coupon to be used in Fare calculations. required - **no**

\
**Examples**

*curl*
```
curl -X POST "https://sb-env.armadillo.pub/api/v1/fares" \
-H "Authorization: Bearer MYaccesstoken123" \
-H "Idempotency-Key: f8b3c7a2-9c5d-4e3b-8a4e-123456abcdef" \
-H "Content-Type: application/json" \
-d '{
	"passenger_id": "P38475",
	"pickup": {	"lat": 35.365730, "long": -120.849863 },
	"dropoff": { "lat": 35.359211, "long": -120.846176 },
	"coupon_id": "PRO9877GGT"
}'
``` 

\
*Node|Fetch*

```js
import fetch from 'node-fetch';

fetch('https://sb-env.armadillo.pub/api/v1/fares', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer MYaccesstoken123',
    'Idempotency-Key': 'f8b3c7a2-9c5d-4e3b-8a4e-123456abcdef',
    'Content-Type': 'application/json'
  },
  // body: '{\n\t"passenger_id": "P38475",\n\t"pickup": {\t"lat": 35.365730, "long": -120.849863 },\n\t"dropoff": { "lat": 35.359211, "long": -120.846176 },\n\t"coupon_id": "PRO9877GGT"\n}',
  body: JSON.stringify({
    'passenger_id': 'P38475',
    'pickup': {
      'lat': 35.36573,
      'long': -120.849863
    },
    'dropoff': {
      'lat': 35.359211,
      'long': -120.846176
    },
    'coupon_id': 'PRO9877GGT'
  })
});
```


### Response

```json
201 Created
{
	"fare_id": "FAR88763",
	"fare_status": "created",
	"fare_calc": {
		"base_amount": 17.40,
		"coupon_id": "PRO9877GGT",
		"discount_amount": 7.00,
		"final_amount": 10.40,
		"currency": "ED"
	},
	"valid_til": "2074-07-15T09:22:51Z"
}
```

- `fare_id` (string) - ID of the created Fare.
- `fare_status` (string) - Status of the created Fare. Possible values:
`created`, `rejected`, `initiated`, `released`, `processed`, `partial_refund`, `refunded`, `failed`. For more details on Fares, refer to Armadillo API glossary.
- `fare_calc` (object) - Fare estimate calculation details.

	- `base_amount` (number|float) - Fare estimate without discounts.
	- `coupon_id` (string/null) - ID of the Coupon applied to the `base_amount`. Will be `null` if none.
	- `discount_amount` (number|float) - Actual amount removed by the applied Coupon, if any. 
	- `final_amount` (number|float) - Final Fare estimate with the discounts. rounded according to the currency rounding rules, and sent to PSP for processing. must be ≥0. if >0, must match the transaction_data.amount within the PSP precision tolerance of ±0.01.
	- `currency` (string) - Currency that the Fare estimate was calculated in.
- `valid_til` (string) - Fare expiration timestamp, ISO 8601.

\
**Errors**

| Status | Message                   | Details                                                                                                                                                                     |
| ------ | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 400    | "required fields missing" | Request missing required fields. Include all required fields in the request and try again.                                                                                  |
| 400    | "malformed values"        | Request contains malformed data (coordinates, IDs, idempotency key, etc.). Ensure all values fit the relevant requirements and try again.                                   |
| 401    | "invalid token"           | Passed authorization token is missing, expired, or invalid.                                                                                                                 |
| 403    | "forbidden"               | Passed `passenger_id` exists but is inactive or blocked from the requested action.                                                                                          |
| 409    | "target conflict"         | Idempotency-Key reused with a different payload.                                                                                                                            |
| 422    | "invalid coordinates"     | Trip distance requirements not met (pickup and dropoff are outside the serviceable area). Make sure the provided coordinates are within the serviceable area and try again. |
| 422    | "failed_coupon"           | Coupon was not applied due to one or more reasons: Coupon inactive or expired; Passenger not eligible.                                                                      |
| 500    | "internal server error"   | Internal error of unknown nature (uncaught exceptions, DB connection failures, etc.).                                                                                       |
| 503    | "service unavailable"     | One or more services are not available (distance calculation, fare estimation, etc.). Refer to `https://status.armadillo.pub` and try again.                                |




# Submit Trip

**`1.1`**

`POST` /trips

Submit a previously estimated trip as a Passenger.

### Request

**Authorization**

Bearer token with any relevant scope.

\
**Path params**

none

\
**Query params**

none

\
**Headers**

- Idempotency-Key (string) - Ensures the request is processed only once. required - **yes**

\
**Body**

- `passenger_id` (string) - Passenger identifier. required - **yes**
- `fare_id` (string) - ID of the associated Fare. required - **yes**

\
**Examples**

*curl*

```
curl -X POST "https://sb-env.armadillo.pub/api/v1/trips" \
-H "Authorization: Bearer MYaccesstoken123" \
-H "Idempotency-Key: f8b3c7a2-9c5d-4e3b-8a4e-123456abcdef" \
-H "Content-Type: application/json" \
-d '{
	"passenger_id": "P38475",
	"fare_id": "FAR88763"
}'
``` 
\
*Node|Fetch*

```js
import fetch from 'node-fetch';

fetch('https://sb-env.armadillo.pub/api/v1/trips', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer MYaccesstoken123',
    'Idempotency-Key': 'f8b3c7a2-9c5d-4e3b-8a4e-123456abcdef',
    'Content-Type': 'application/json'
  },
  // body: '{\n\t"passenger_id": "P38475",\n\t"fare_id": "FAR88763"\n}',
  body: JSON.stringify({
    'passenger_id': 'P38475',
    'fare_id': 'FAR88763'
  })
});
```


### Responses

```json
201 Created
{
	"trip_id": "T8-7354-15D",
	"trip_status": "submitted"
}
```

- `trip_id` (string) - ID of the created Trip.
- `trip_status` (string) - status of the created Trip.

\
**Errors**

| Status | Message                   | Details                                                                                                                                      |
| ------ | ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 400    | "required fields missing" | Request missing required fields. Include all required fields in the request and try again.                                                   |
| 400    | "malformed values"        | Request contains malformed data (coordinates, IDs, idempotency key, etc.). Ensure all values fit the relevant requirements and try again.    |
| 401    | "invalid token"           | Passed authorization token is missing, expired, or invalid.                                                                                  |
| 403    | "forbidden"               | Passed `passenger_id` exists but is inactive or blocked from the requested action.                                                           |
| 404    | "not found"               | Passed `passenger_id` or `fare_id` doesn't exist.                                                                                            |
| 409    | "target conflict"         | Idempotency-Key reused with a different payload.                                                                                             |
| 409    | "invalid fare state"      | Failed to associate Fare with the Trip. Fare is rejected, expired, linked to a different Passenger or Trip.                                  |
| 500    | "internal server error"   | Internal error of unknown nature (uncaught exceptions, DB connection failures, etc.).                                                        |
| 503    | "service unavailable"     | One or more services are not available (trip matching, internal orchestration, etc.). Refer to `https://status.armadillo.pub` and try again. |




# Accept Trip

**`1.1`**

`PATCH` /trips/{trip_id}

Accept a submitted Trip as a Driver.


### Request

**Authorization**

Bearer token with any relevant scope.

\
**Path params**

- `trip_id` (string) - ID of the accepted Trip. required - **yes**

\
**Query params**

none

\
**Headers**

- Idempotency-Key (string) - Ensures the request is processed only once. required - **yes**

\
**Body**

- `driver_id` (string) - ID of the Driver accepting the Trip. required - **yes**

\
**Examples**

*curl*

```
curl -X PATCH "https://sb-env.armadillo.pub/api/v1/trips/T8-7354-15D" \
-H "Authorization: Bearer MYauthorizationtoken123" \
-H "Idempotency-Key: f8b3c7a2-9c5d-4e3b-8a4e-123456abcdef" \
-H "Content-Type: application/json" \
-d '{
	"driver_id": "D10154"
}'
```
\
*Node|Fetch*

```js
import fetch from 'node-fetch';

fetch('https://sb-env.armadillo.pub/api/v1/trips/T8-7354-15D', {
  method: 'PATCH',
  headers: {
    'Authorization': 'Bearer MYauthorizationtoken123',
    'Idempotency-Key': 'f8b3c7a2-9c5d-4e3b-8a4e-123456abcdef',
    'Content-Type': 'application/json'
  },
  // body: '{\n\t"driver_id": "D10154"\n}',
  body: JSON.stringify({
    'driver_id': 'D10154'
  })
});
```


### Response

```json
200 OK
{
	"trip_id": "T8-7354-15D",
	"trip_status": "accepted",
	"driver_id": "D10154"
}
```

- `trip_id` (string) - ID of the accepted Trip.
- `trip_status` (string) - status of the accepted Trip.
- `driver_id` (string) - ID of the Driver who accepted the Trip.

\
**Errors**

| Status | Message                   | Details                                                                                                                                      |
| ------ | ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 400    | "required fields missing" | Request missing required fields. Include all required fields in the request and try again.                                                   |
| 400    | "malformed values"        | Request contains malformed data (IDs, idempotency key, etc.). Ensure all values fit the relevant requirements and try again.                 |
| 401    | "invalid token"           | Passed authorization token is missing, expired, or invalid.                                                                                  |
| 403    | "forbidden"               | Passed `driver_id` exists but is inactive or blocked from the requested action.                                                              |
| 404    | "not found"               | Passed `driver_id` or `trip_id` doesn't exist.                                                                                               |
| 409    | "target conflict"         | Idempotency-Key reused with a different payload.                                                                                             |
| 409    | "trip already accepted"   | Trip has been already accepted by a Driver, so it cannot be associated with the passed `driver_id`.                                          |
| 409    | "trip canceled"           | Trip has been canceled, so it cannot be associated with the passed `driver_id`.                                                              |
| 409    | "trip closed"             | Trip has been closed, so it cannot be associated with the passed `driver_id`.                                                                |
| 409    | "cab unavailable"         | Cab associated with the `driver_id` is outside the service area or offline.                                                                  |
| 409    | "driver busy"             | Passed `driver_id` is associated with another ongoing Trip. Close the ongoing Trip and try again.                                            |
| 500    | "internal server error"   | Internal error of unknown nature (uncaught exceptions, DB connection failures, etc.).                                                        |
| 503    | "service unavailable"     | One or more services are not available (trip matching, internal orchestration, etc.). Refer to `https://status.armadillo.pub` and try again. |



# Rate Trip

**`1.1`**

`POST` /ratings

Rate a "closed" Trip as a Passenger.

### Request

**Authorization**

Bearer token with any relevant scope.

\
**Path params**

none

\
**Query params**

none

\
**Headers**

- Idempotency-Key (string) - Ensures the request is processed only once. required - **yes**

\
**Body**

- `passenger_id` (string) - ID of the Passenger who rated the Trip. required - **yes**
- `trip_id` (string) - ID of the rated Trip. required - **yes**
- `rating_value` (string) - actual Trip rating. required - **yes**

\
**Examples**

*curl*

```
curl -X POST "https://sb-env.armadillo.pub/api/v1/ratings" \
-H "Authorization: Bearer MYaccesstoken123" \
-H "Idempotency-Key: f8b3c7a2-9c5d-4e3b-8a4e-123456abcdef" \
-H "Content-Type: application/json" \
-d '{
	"passenger_id": "P38475",
	"trip_id": "T8-7354-15D",
	"rating_value": 5
}'
```

\
*Node|Fetch*

```js
import fetch from 'node-fetch';

fetch('https://sb-env.armadillo.pub/api/v1/ratings', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer MYaccesstoken123',
    'Idempotency-Key': 'f8b3c7a2-9c5d-4e3b-8a4e-123456abcdef',
    'Content-Type': 'application/json'
  },
  // body: '{\n\t"passenger_id": "P38475",\n\t"trip_id": "T8-7354-15D",\n\t"rating_value": 5\n}',
  body: JSON.stringify({
    'passenger_id': 'P38475',
    'trip_id': 'T8-7354-15D',
    'rating_value': 5
  })
});
```

### Response

```json
201 Created
{
	"rating_id": "R21094",
	"rating_status": "accepted"
}
```

- `rating_id` (string) - ID of the created Rating.
- `rating_status` (string) - Status of the created Rating.


\
**Errors**

| Status | Message                   | Details                                                                                                                                                 |
| ------ | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 400    | "required fields missing" | Request missing required fields. Include all required fields in the request and try again.                                                              |
| 400    | "malformed values"        | Request contains malformed data (IDs, idempotency key, etc.). Ensure all values fit the relevant requirements and try again.                            |
| 401    | "invalid token"           | Passed authorization token is missing, expired, or invalid.                                                                                             |
| 403    | "forbidden"               | Passed `passenger_id` exists but is inactive or blocked from the requested action.<br><br>Passed `trip_id` is associated with different `passenger_id`. |
| 404    | "not found"               | Passed `passenger_id` or `trip_id` doesn't exist.                                                                                                       |
| 409    | "target conflict"         | Idempotency-Key reused with a different payload.                                                                                                        |
| 409    | "invalid trip state"      | Possible reasons:<br>- The Trip is not "closed" or has already been rated;<br>- The Trip rating window expired.                                         |
| 500    | "internal server error"   | Internal error of unknown nature (uncaught exceptions, DB connection failures, etc.).                                                                   |
| 503    | "service unavailable"     | One or more services are not available (trip matching, internal orchestration, etc.). Refer to `https://status.armadillo.pub` and try again.            |




# Get Trips
**`1.1`**

`GET` /admin/trips

Get Trip records as an Admin. Use optional filters to narrow down the results.


### Request

**Authorization**

Bearer token with `admin.read` scope or higher.

\
**Path params**

none

\
**Query params**

- `trip_id` (string) - Return a specific Trip. If specified, other filters are ignored. required - **no**
- `driver_id` (string) - Filter the response by Driver ID. required - **no**
- `cab_id` (string) - Filter the response by Cab ID. required - **no**
- `passenger_id` (string) - Filter the response by Passenger ID. required - **no**
- `trip_status` (string) - Filter the response by Trip status. required - **no**
- `created_from` (string) - Beginning of the date range to get Trips for. required - **no**
- `created_to` (string) - End of the date range to get Trips for. required - **no**
- `include` (string) - List related resources to include in the response. Separate by comma. Possible values: `driver`, `passenger`, `cab`, `fare`, `rating`. required - **no**
- `sort` (string) - Sort the items in the response by one or more parameters in ascending (`asc`) or descending (`desc`) order. Example values: `created_at:desc` (default), `driver_id:asc`. required - **yes**
- `limit` (integer) - Limit the number of items in the response. `50` by default, `200` max. required - **no**
- `cursor` (string) - Opaque pagination token that represents a position in a response set. If returned in a response, include it in the query **as is** to get the next page of results. Do not construct or modify the token manually. required - **no**

>[!NOTE] Note:
> - Date range of a request may not exceed 31 days.
> - The API may evolve toward a generic `filter` pattern in future iterations.

\
**Headers**

none

\
**Body**

none

\
**Example**

*curl*

```
curl "https://sb-env.armadillo.pub/api/v1/admin/trips?driver_id=D10154&trip_status=closed&created_from=2024-01-01T00:00:00Z&created_to=2024-01-10T00:00:00Z&include=passenger&sort=created_at:desc&limit=50" \
-H "Authorization: Bearer MYaccesstoken123"
```
\
*Node|Fetch*

```js
import fetch from 'node-fetch';

fetch('https://sb-env.armadillo.pub/api/v1/admin/trips?driver_id=D10154&trip_status=closed&created_from=2024-01-01T00:00:00Z&created_to=2024-01-10T00:00:00Z&include=passenger&sort=created_at:desc&limit=50', {
  headers: {
    'Authorization': 'Bearer MYaccesstoken123'
  }
});
```

### Response

```json
200 OK
{
	"data": [
		{
			"trip_id": "T8-7354-15D",
			"driver_id": "D10154",
			"cab_id": "C10186",
			"passenger_id": "P37475",
			"passenger": {
				"passenger_id": "P37475",
				"passenger_status": "active",
				"first_name": "Jane",
				"last_name": "Doe",
				"joined_at": "2074-07-15T09:17:51Z",
				"trips_closed": 18,
				"passenger_rating": 4.45
			},
			"fare_id": "FAR88763",
			"trip_status": "closed",
			"trip_statuses": [
				{
					"status_name": "submitted",
					"set_at": "2024-01-02T12:14:32Z",
					"set_lat": 35.365730,
					"set_long": -120.849863
				},
				{
					"status_name": "accepted",
					"set_at": "2024-01-02T12:16:05Z",
					"set_lat": 35.365730,
					"set_long": -120.849863
				},
				{
					"status_name": "at_pickup",
					"set_at": "2024-01-02T12:27:43Z",
					"set_lat": 35.365730,
					"set_long": -120.849863
				},
				{
					"status_name": "started",
					"set_at": "2024-01-02T12:29:02Z",
					"set_lat": 35.365730,
					"set_long": -120.849863
				},
				{
					"status_name": "at_drop",
					"set_at": "2024-01-02T12:50:22Z",
					"set_lat": 35.365730,
					"set_long": -120.849863
				},
				{
					"status_name": "closed",
					"set_at": "2024-01-02T12:52:37Z",
					"set_lat": 35.365730,
					"set_long": -120.849863
				}
			],
			"distance": {
				"estimated": 4.12,
				"actual": 4.12
			},
			"rating": {
				"rating_id": "R21094"
			}
		}
	],
	"next_cursor": "eyJjcmVhdGVkX2F0IjoiMjAyNC0wNS0xNVQxMDozMTo0MVoifQ=="
}
```

- `data` (array) - List of objects matching the request. Returns empty if no matching records found.

	- `trip_id` (string) - ID of the Trip.
	- `driver_id` (string) - ID of the Driver associated with the Trip.
	- `cab_id` (string) - ID of the Cab associated with the Trip.
	- `passenger_id` (string) - ID of the Passenger associated with the Trip.
	- `passenger` (object) - **Summary** of the Passenger associated with the Trip. Included optionally via query filters.
		- `passenger_id` (string) - ID of the Passenger associated with the Trip.
		- `passenger_status` (string) - Status of the Passenger associated with the Trip. Possible values: `active`, `deactivated`.
		- `first_name` (string) - Passenger's first name.
		- `last_name` (string) - Passenger's last name.
		- `joined_at` (string) - Passenger's registration timestamp.
		- `trips_closed` (integer) - Total number of Trips the Passenger closed.
		- `passenger_rating` (number|float) - Passenger's rating among the Drivers.
	- `fare_id` (string) - ID of the Fare associated with the Trip.
	- `trip_status` (string) - Latest status assigned to the Trip. Possible values: `submitted`, `accepted`, `at_pickup`, `started`, `at_drop`, `closed`, `canceled`.
	- `trip_statuses` (array) - List of all statuses assigned to the Trip through its lifecycle.

		- `status_name` (string) - Status assigned to the Trip. Possible values: `submitted`, `accepted`, `at_pickup`, `started`, `at_drop`, `closed`, `canceled`.
		- `set_at` (string) - Status assignment timestamp.
		- `set_lat` (number|float) - Latitude of the the status assignment point.
		- `set_long` (number|float) - Longitude of the the status assignment point.
		- `canceled_by` (string) - Party that canceled the Trip. Present only if `data.trip_statuses.status_name` = `canceled`. Possible values: `system`, `passenger`, `driver`, `dispatch`.
		- `cancellation_reason` (string) - Reason for the Trip cancellation. Present only if `data.trip_statuses.status_name` = `canceled`. Timed out trips are closed automatically with reason `timeout`; manual cancellations must have a reason. Example values: `Low passenger rating`, `Cab stuck in traffic`.
	- `distance` (object) - Data about Trip distance.
		- `estimated` (number|float) - Trip distance calculated from the difference between the set pickup and drop-off points.
		- `actual` (number|float) - Trip distance calculated from the difference between the pickup point and the coordinates where the Trip received `at_drop`, `closed`, or `canceled` status.
	- `rating` (object) - Data about the Trip rating.
		- `rating_id` (string) - ID of the Rating associated with the Trip.
- `next_cursor` (string) - Pagination token. Present only if more matching records are available. Pass in the next query **as is** in `cursor` to get the next page of results. Do not construct or modify manually.

\
**Errors**

| Status | Message                 | Details                                                                                                                         |
| ------ | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| 400    | "bad request"           | Conflicting or unsupported query parameters provided.                                                                           |
| 400    | "malformed values"      | Request contains malformed data (IDs, timestamps, etc.). Ensure all values fit the relevant requirements and try again.         |
| 401    | "invalid token"         | Passed authorization token is missing, expired, or invalid.                                                                     |
| 403    | "forbidden"             | Passed authorization token is valid, but user lacks required scope or is blocked from executing this action.                    |
| 422    | "unprocessable request" | Request is logically impossible (e.g., date range is reversed).                                                                 |
| 429    | "too many requests"     | Rate limit exceeded. Wait a short period of time and try again. Repeated retries without delay may extend the request blocking. |
| 500    | "internal server error" | Internal error of unknown nature (uncaught exceptions, DB connection failures, etc.).                                           |
| 503    | "service unavailable"   | One or more services are not available. Refer to `https://status.armadillo.pub` and try again.                                  |
