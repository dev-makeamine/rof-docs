# Subscription Webhook API (v2)

## Get Subscriptions

Retrieve all subscriptions (`PROCESSING`, `ACTIVE`, `CANCELLED` and `PAUSED`) for a user using their phone number.

### Endpoint

```http
GET /webhook/v2/subscription?phone=<PHONE>
```

### Query Parameters

| Parameter | Type     | Required | Description                                                                                               |
| --------- | -------- | -------- | --------------------------------------------------------------------------------------------------------- |
| `phone`   | `string` | Yes      | User phone number used for login. Format must follow `<COUNTRY_CODE><PHONE>`, for example `917827676141`. |

### Example Success Response

```json
{
  "message": "Subscription found for user +917827676141",
  "subscriptions": [
    {
      "subscriptionId": "subscription_53195c-bca5-4d88-9017-e59b74a628bf",
      "refId": "#SUB-1",
      "type": "FLEXIBLE",
      "status": "ACTIVE",
      "cycleDuration": 7,
      "nextOrder": "2026-09-11T10:50:50.029Z",
      "lastPausedAt": null,
      "cancelledAt": null,
      "createdAt": "2026-09-04T10:50:50.161Z",
      "updatedAt": "2026-09-04T10:50:50.161Z",
      "userIdentifier": "+917827676141",
      "idempotencyKey": "subscription_3716afc4-...:7:variant_814fa226-...",
      "isGifting": false,
      "gifting": {
        "reason": "",
        "message": ""
      },
      "shipping": {
        "tag": "SHIPPING",
        "label": "HOME",
        "firstName": "Damanjeet",
        "lastName": "Singh",
        "phone": "+917827676141",
        "email": "damanjeetsingh434@gmail.com",
        "address": "WZ-26",
        "street": "Street-19, Sant Garh",
        "city": "Tilak Nagar",
        "state": "Delhi",
        "stateCode": "DL",
        "zip": "110018",
        "country": "India",
        "countryCode": "IN",
        "county": null,
        "courier": "",
        "publicId": "post_0cd15c4a-9044-4852-b7cc-befa3ec856b5",
        "addressId": "96965000041126011",
        "customerId": "96965000041126003",
        "attention": "",
        "customLabel": ""
      },
      "billing": {
        "firstName": "Damanjeet",
        "lastName": "Singh",
        "phone": "+917827676141",
        "email": "damanjeetsingh434@gmail.com",
        "address": "WZ-26",
        "street": "Street-19, Sant Garh",
        "city": "Tilak Nagar",
        "state": "Delhi",
        "stateCode": "DL",
        "zip": "110018",
        "country": "India",
        "countryCode": "IN",
        "addressId": "96965000041126009"
      },
      "items": [
        {
          "sku": "103A20030801E04",
          "name": "Salad Test [1000g]",
          "price": 100,
          "quantity": 1,
          "inStock": true,
          "isPublished": true,
          "availableInStock": 100
        }
      ]
    }
  ]
}
```

### Success Response (with no subscriptions)

```json
{
  "message": "No subscription found for this user"
}
```

### Error Response

```json
{
  "message": "Invalid phone number"
}
```

```json
{
  "message": "Phone not provided"
}
```

---

## Update Subscription Status

Update the status of a subscription to `PAUSED` or `CANCELLED`.

If the subscription is currently being processed, the requested status change is temporarily stored in Redis and will be applied after the current processing cycle completes.

### Endpoint

```http
PATCH /webhook/v2/subscription/<SUBSCRIPTION_ID>
```

### Path Parameters

| Parameter        | Type     | Required | Description                                                     |
| ---------------- | -------- | -------- | --------------------------------------------------------------- |
| `subscriptionId` | `string` | Yes      | The internal subscription ID used to identify the subscription. |

### Request Body

```json
{
  "status": "PAUSED"
}
```

or

```json
{
  "status": "CANCELLED"
}
```

### Allowed Status Values

| Status      | Description                          |
| ----------- | ------------------------------------ |
| `PAUSED`    | Temporarily pause the subscription.  |
| `CANCELLED` | Permanently cancel the subscription. |

Only `PAUSED` and `CANCELLED` can be submitted through this endpoint.

---

## Subscription Status Change Behaviour

The status change behaves differently depending on the subscription's current status.

### Case 1: Subscription is `ACTIVE`

If the subscription is currently `ACTIVE`, the requested status is applied immediately.

#### Request

```http
PATCH /webhook/v2/subscription/<SUBSCRIPTION_ID>
```

```json
{
  "status": "PAUSED"
}
```

#### Response

```json
{
  "message": "Subscription changes processed"
}
```

HTTP Status:

```http
200 OK
```

The subscription will immediately become:

```text
ACTIVE → PAUSED
```

The same applies when requesting cancellation:

```text
ACTIVE → CANCELLED
```

---

### Case 2: Subscription is `PAUSED`

If the subscription is already `PAUSED`, a new `PAUSED` request is processed immediately.

If `CANCELLED` is requested, the subscription is immediately changed to:

```text
PAUSED → CANCELLED
```

#### Request

```json
{
  "status": "CANCELLED"
}
```

#### Response

```json
{
  "message": "Subscription changes processed"
}
```

HTTP Status:

```http
200 OK
```

---

### Case 3: Subscription is `CANCELLED`

A subscription that is already `CANCELLED` cannot be meaningfully moved back to `PAUSED` through this endpoint.

The service will currently process the request because the subscription is not `PROCESSING`, so callers should treat `CANCELLED` as the terminal state.

Recommended client behaviour:

```text
CANCELLED → PAUSED
```

should not be requested.

---

### Case 4: Subscription is `PROCESSING`

When the subscription is currently `PROCESSING`, the status is **not changed immediately**.

Instead, the requested status is stored in Redis against the subscription ID.

For example:

```text
PROCESSING
    ↓
PATCH { "status": "PAUSED" }
    ↓
Redis
    ↓
PAUSED
```

The subscription remains:

```text
PROCESSING
```

until the current processing cycle finishes.

#### Response

```json
{
  "message": "Changes will be processed shortly"
}
```

HTTP Status:

```http
200 OK
```

---

## Cancellation Priority

`CANCELLED` has higher priority than `PAUSED`.

If a subscription is currently `PROCESSING` and a `PAUSED` request has already been queued:

```text
PROCESSING
    ↓
PAUSED requested
    ↓
Redis: PAUSED
```

and a subsequent request asks for cancellation:

```text
PROCESSING
    ↓
CANCELLED requested
    ↓
Redis: CANCELLED
```

the queued state becomes:

```text
CANCELLED
```

If `CANCELLED` is already queued and another `PAUSED` request is received, the `PAUSED` request is ignored.

### Example

First request:

```json
{
  "status": "PAUSED"
}
```

Response:

```json
{
  "message": "Changes will be processed shortly"
}
```

Second request:

```json
{
  "status": "CANCELLED"
}
```

Response:

```json
{
  "message": "Changes will be processed shortly"
}
```

Final queued state:

```text
CANCELLED
```

---

## Case 5: `PROCESSING` + `CANCELLED` Already Queued

If the subscription is `PROCESSING` and Redis already contains `CANCELLED`, another `PAUSED` request will not override the cancellation.

### Request

```json
{
  "status": "PAUSED"
}
```

### Response

```json
{
  "message": "Subscription already marked cancelled...skipping status change"
}
```

HTTP Status:

```http
200 OK
```

The subscription remains:

```text
PROCESSING
```

and the queued status remains:

```text
CANCELLED
```

---

## Invalid Status

Only `PAUSED` and `CANCELLED` are accepted.

For example, the following request is invalid:

```json
{
  "status": "ACTIVE"
}
```

The service returns:

```json
{
  "message": "Only CANCELLED or PAUSED status changes can be queued"
}
```

HTTP Status:

```http
400 Bad Request
```

The same applies to other unsupported values such as:

```json
{
  "status": "PROCESSING"
}
```

```json
{
  "status": "SUCCESSFUL"
}
```

---

## Missing Status

If the request body does not contain `status`:

### Request

```json
{}
```

### Response

```json
{
  "message": "No status found in body"
}
```

HTTP Status:

```http
400 Bad Request
```

---

## Subscription Not Found

If the supplied `subscriptionId` does not exist:

### Request

```http
PATCH /webhook/v2/subscription/<INVALID_SUBSCRIPTION_ID>
```

### Response

```json
{
  "message": "Subscription not found"
}
```

HTTP Status:

```http
400 Bad Request
```

---

## Server Error

If an unexpected error occurs while processing the request:

```json
{
  "message": "Failed to update subscription status"
}
```

HTTP Status:

```http
500 Internal Server Error
```

---

## Status Change Flow

The overall flow can be summarized as:

```text
                    PATCH STATUS
                         │
                         ▼
              Is status valid?
                 /           \
               NO             YES
               │               │
               ▼               ▼
             400          Find subscription
                               │
                               ▼
                       Is subscription
                        PROCESSING?
                         /         \
                       NO           YES
                       │             │
                       ▼             ▼
                  Update DB      Check Redis
                       │             │
                       │             ▼
                       │      Is CANCELLED queued?
                       │          /       \
                       │        YES       NO
                       │        │           │
                       │        ▼           ▼
                       │      Ignore      Set status
                       │                    in Redis
                       ▼
                     200
```

## Response Summary

| Situation                                      | HTTP Status | Response Message                                                 |
| ---------------------------------------------- | ----------- | ---------------------------------------------------------------- |
| Valid status, subscription not processing      | `200`       | `Subscription changes processed`                                 |
| Valid status, subscription processing          | `200`       | `Changes will be processed shortly`                              |
| `CANCELLED` already queued, `PAUSED` requested | `200`       | `Subscription already marked cancelled...skipping status change` |
| Missing `status`                               | `400`       | `No status found in body`                                        |
| Invalid status                                 | `400`       | `Only CANCELLED or PAUSED status changes can be queued`          |
| Subscription not found                         | `400`       | `Subscription not found`                                         |
| Unexpected server error                        | `500`       | `Failed to update subscription status`                           |
