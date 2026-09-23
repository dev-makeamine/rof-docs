# Red Otter Farms API Documentation

This repository contains documentation related to APIs provided by the ROF Website Team.

## Available APIs

### Variant API

Documentation for retrieving and updating product variants using their SKU.

- **Get Variant**
- **Update Variant**
- **API Authentication**
- **Request and Response Examples**
- **Validation Error Responses**

👉 **[View Variant API Documentation](./api/variant.md)**

### Subscriptions API

Documentation for retrieving and updating subscriptions using their id.

- **Get Subscriptions**
- **Update Subscription Status**
- **API Authentication**
- **Request and Response Examples**
- **Validation Error Responses**

👉 **[View Subscription API Documentation](./api/subscription.md)**

---

## Available Documentations

### Order Docs

Documentation for understanding different order types and how razorpay payment can be verified using the key values sent by website

👉 **[View Order Documentation](./docs/order.md)**

---

## Documentation Structure

```text
.
├── README.md
└── api/
    └── variant-v2.md
    └── subscription.md
└── docs/
    └── order.md

```

## Environments

| Environment | Base URL                        |
| ----------- | ------------------------------- |
| Production  | `https://redotterfarms.health`  |
| Staging     | `https://temp.redotterfarms.in` |

## Authentication

All API requests require authentication.

### API (v2)

The API (v2) uses an API key for authentication.

```http
x-api-key: YOUR_API_KEY
```

### Legacy API

Legacy API endpoints use `API_SECRET` for authentication.

```http
API_SECRET: YOUR_API_SECRET
```