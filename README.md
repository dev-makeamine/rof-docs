# Red Otter Farms API Documentation

This repository contains documentation related to APIs provided by the ROF Website Team.

## Available APIs

### Variant API (v2)

Documentation for retrieving and updating product variants using their SKU.

* **Get Variant**
* **Update Variant**
* **API Authentication**
* **Request and Response Examples**
* **Validation Error Responses**

👉 **[View Variant API (v2) Documentation](./api/variant-v2.md)**

---

## Documentation Structure

```text
.
├── README.md
└── api/
    └── variant-v2.md
```

## Environments

| Environment | Base URL                        |
| ----------- | ------------------------------- |
| Production  | `https://redotterfarms.health`  |
| Staging     | `https://temp.redotterfarms.in` |

## Authentication

All API requests require authentication.

### Variant API (v2)

The Variant API (v2) uses an API key for authentication.

```http
x-api-key: YOUR_API_KEY
```

### Legacy API

Legacy API endpoints use `API_SECRET` for authentication.

```http
API_SECRET: YOUR_API_SECRET
```

For complete endpoint details, request parameters, request examples, responses, and error handling, see the **[Variant API (v2) documentation](./api/variant-v2.md)**.
