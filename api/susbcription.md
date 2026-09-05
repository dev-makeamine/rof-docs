# Subscription Webhook API (v2)



## Get Subscriptions

Retrieve all subscriptions (PROCESSING, ACTIVE, CANCELLED and PAUSED) for a user using their phone number

### Endpoint

```http
GET /webhook/v2/subscription?phone=<PHONE>
```

### Path Parameters

| Parameter | Type     | Required | Description                                                                                     |
| --------- | -------- | -------- | ----------------------------------------------------------------------------------------------- |
| `phone`   | `string` | Yes      | User phone number used for login format must follow <COUNTRY_CODE><PHONE>, for eg: 917827676141 |

### Example Success Response

```json
{
  "message": "Subscription found for user [object Object]",
  "subscriptions": [
    {
      "publicId": "subscription_53195c-bca5-4d88-9017-e59b74a628bf",
      "type": "FLEXIBLE",
      "userIdentifier": "+917827676141",
      "shipping": {
        "tag": "SHIPPING",
        "zip": "110018",
        "city": "Tilak Nagar",
        "email": "damanjeetsingh434@gmail.com",
        "label": "HOME",
        "phone": "+917827676141",
        "state": "Delhi",
        "county": null,
        "street": "Street-19, Sant Garh",
        "address": "WZ-26",
        "country": "India",
        "courier": "",
        "lastName": "Singh",
        "publicId": "post_0cd15c4a-9044-4852-b7cc-befa3ec856b5",
        "addressId": "96965000041126011",
        "attention": "",
        "firstName": "Damanjeet",
        "stateCode": "DL",
        "customerId": "96965000041126003",
        "countryCode": "IN",
        "customLabel": ""
      },
      "billing": {
        "zip": "110018",
        "city": "Tilak Nagar",
        "email": "damanjeetsingh434@gmail.com",
        "phone": "+917827676141",
        "state": "Delhi",
        "street": "Street-19, Sant Garh",
        "address": "WZ-26",
        "country": "India",
        "lastName": "Singh",
        "addressId": "96965000041126009",
        "firstName": "Damanjeet",
        "stateCode": "DL",
        "countryCode": "IN"
      },
      "gifting": {
        "reason": "",
        "message": ""
      },
      "isGifting": false,
      "status": "ACTIVE",
      "cycleDuration": 7,
      "nextOrder": "2026-09-11T10:50:50.029Z",
      "createdAt": "2026-09-04T10:50:50.161Z",
      "updatedAt": "2026-09-04T10:50:50.161Z",
      "items": [
        {
          "sku": "103A20030801E04",
          "name": "Salad Test [1000g]",
          "price": 100,
          "inStock": true,
          "isPublished": true,
          "availableInStock": 100,
          "quantity": 1
        }
      ]
    },
    {
      "publicId": "subscription_td4a4b8ca-c510-4af6-b9dd-3055b22daebb",
      "type": "FLEXIBLE",
      "userIdentifier": "+917827676141",
      "shipping": {
        "tag": "SHIPPING",
        "zip": "110018",
        "city": "Tilak Nagar",
        "email": "damanjeetsingh434@gmail.com",
        "label": "HOME",
        "phone": "+917827676141",
        "state": "Delhi",
        "county": null,
        "street": "Street-19, Sant Garh",
        "address": "WZ-26",
        "country": "India",
        "courier": "",
        "lastName": "Singh",
        "publicId": "post_0cd15c4a-9044-4852-b7cc-befa3ec856b5",
        "addressId": "96965000041126011",
        "attention": "",
        "firstName": "Damanjeet",
        "stateCode": "DL",
        "customerId": "96965000041126003",
        "countryCode": "IN",
        "customLabel": ""
      },
      "billing": {
        "zip": "110018",
        "city": "Tilak Nagar",
        "email": "damanjeetsingh434@gmail.com",
        "phone": "+917827676141",
        "state": "Delhi",
        "street": "Street-19, Sant Garh",
        "address": "WZ-26",
        "country": "India",
        "lastName": "Singh",
        "addressId": "96965000041126009",
        "firstName": "Damanjeet",
        "stateCode": "DL",
        "countryCode": "IN"
      },
      "gifting": {
        "reason": "",
        "message": ""
      },
      "isGifting": false,
      "status": "ACTIVE",
      "cycleDuration": 1,
      "nextOrder": "2026-09-05T10:50:50.029Z",
      "createdAt": "2026-09-04T10:50:50.134Z",
      "updatedAt": "2026-09-04T10:50:50.134Z",
      "items": [
        {
          "sku": "103A20030801E04",
          "name": "Salad Test [1000g]",
          "price": 100,
          "inStock": true,
          "isPublished": true,
          "availableInStock": 100,
          "quantity": 1
        }
      ]
    },
    {
      "publicId": "subscription_61231e04-0c5e-446a-abe5-8f28b32f9a96",
      "type": "FLEXIBLE",
      "userIdentifier": "+917827676141",
      "shipping": {
        "tag": "SHIPPING",
        "zip": "110018",
        "city": "Tilak Nagar",
        "email": "damanjeetsingh434@gmail.com",
        "label": "HOME",
        "phone": "+917827676141",
        "state": "Delhi",
        "county": null,
        "street": "Street-19, Sant Garh",
        "address": "WZ-26",
        "country": "India",
        "courier": "",
        "lastName": "Singh",
        "publicId": "post_0cd15c4a-9044-4852-b7cc-befa3ec856b5",
        "addressId": "96965000041126011",
        "attention": "",
        "firstName": "Damanjeet",
        "stateCode": "DL",
        "customerId": "96965000041126003",
        "countryCode": "IN",
        "customLabel": ""
      },
      "billing": {
        "zip": "110018",
        "city": "Tilak Nagar",
        "email": "damanjeetsingh434@gmail.com",
        "phone": "+917827676141",
        "state": "Delhi",
        "street": "Street-19, Sant Garh",
        "address": "WZ-26",
        "country": "India",
        "lastName": "Singh",
        "addressId": "96965000041126009",
        "firstName": "Damanjeet",
        "stateCode": "DL",
        "countryCode": "IN"
      },
      "gifting": {
        "reason": "",
        "message": ""
      },
      "isGifting": false,
      "status": "ACTIVE",
      "cycleDuration": 14,
      "nextOrder": "2026-09-18T10:50:50.029Z",
      "createdAt": "2026-09-04T10:50:50.173Z",
      "updatedAt": "2026-09-04T10:50:50.173Z",
      "items": [
        {
          "sku": "103A20030801E04",
          "name": "Salad Test [1000g]",
          "price": 100,
          "inStock": true,
          "isPublished": true,
          "availableInStock": 100,
          "quantity": 1
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
