# Variant Webhook API (v2)

## Get Variant

Retrieve product information to which the SKU is linked to.

### Endpoint

```http
GET /webhook/v2/variant/{sku}
```

### Path Parameters

| Parameter | Type     | Required | Description                    |
| --------- | -------- | -------- | ------------------------------ |
| `sku`     | `string` | Yes      | SKU of the variant to retrieve |

### Example Success Response

```json
{
  "success": true,
  "message": "Product details found",
  "product": {
    "publicId": "product_b3745e22-a058-4a6a-af1c-f96212139e29",
    "slug": "salad-test",
    "name": "Salad Test",
    "displayName": "Salad Test",
    "summary": "",
    "healthBenefits": [],
    "goodForYou": [],
    "howToStore": ["Nan"],
    "whatsInBox": [],
    "serving": "",
    "minPrice": 0,
    "maxPrice": 0,
    "nutritionalInfo": {
      "Fat (g)": "0g",
      "Fibre (g)": "0g",
      "Iron (mg)": "0mg",
      "Sugar (g)": "0g",
      "Zinc (mg)": "0mg",
      "Bioactives": "0",
      "Copper (mg)": "0mg",
      "Niacin (mg)": "0mg",
      "Protein (g)": "0g",
      "Sodium (mg)": "0mg",
      "Biotin (µg)": "0µg",
      "Calcium (mg)": "0mg",
      "Folate (µg)": "0µg",
      "Energy (kcal)": "0kcal",
      "Lycopene (µg)": "0µg",
      "Magnesium (mg)": "0mg",
      "Manganese (mg)": "0mg",
      "Potassium (mg)": "0mg",
      "Selenium (µg)": "0µg",
      "Vitamin C (mg)": "0mg",
      "Vitamin D (IU)": "0IU",
      "Vitamin E (mg)": "0mg",
      "Phosphorus (mg)": "0mg",
      "Riboflavin (mg)": "0mg",
      "Trace Nutrients": "",
      "Vitamin A (µg)": "0µg",
      "Vitamin B6 (mg)": "0mg",
      "Vitamin K (µg)": "0µg",
      "Carbohydrate (g)": "0g",
      "Vitamin B12 (µg)": "0µg"
    },
    "isPublished": true,
    "isFeatured": false,
    "isDryStore": false,
    "hasSubscription": false,
    "allowFlexibleSubscription": true,
    "flexibleSubscriptionOptions": ["DAILY", "WEEKLY", "BI_WEEKLY"],
    "keywords": [],
    "createdAt": "2026-08-31T07:27:10.752Z",
    "updatedAt": "2026-08-31T10:35:46.453Z",
    "categories": [
      {
        "publicId": "category_698b180b-b9e6-45eb-b9be-1e254f5bf115",
        "name": "sample",
        "displayName": "Sample",
        "slug": "category_sample_c002a0ba3d5e490bbe157c432b5c2d70",
        "description": "",
        "lastUpdatedById": null,
        "quickShop": true,
        "isPublished": true,
        "createdAt": "2026-08-31T07:15:59.156Z",
        "updatedAt": "2026-08-31T07:15:59.156Z"
      }
    ],
    "faqs": [],
    "facts": [],
    "assets": [
      {
        "id": "ffabd347-c6d2-44a5-ab90-1ace0ea935dd",
        "productId": "926ce61a-811a-4c1d-b985-0f3a03a30538",
        "url": "https://red-otter.s3.ap-south-1.amazonaws.com/uploads/images/deb26499-74ce-4636-9a74-28a6c97aa442.webp",
        "thumbnail": "https://red-otter.s3.ap-south-1.amazonaws.com/uploads/images/deb26499-74ce-4636-9a74-28a6c97aa442.webp",
        "type": "IMAGE",
        "position": 0,
        "isPrimary": true
      }
    ],
    "options": [
      {
        "displayName": "Weight",
        "slug": "option_weight_26877b3788d24bd89e9b1643400c6cf8",
        "values": [
          {
            "displayName": "1 Kg",
            "slug": "value_1kg_42533bc335f94d06903a2179119e439d",
            "isDefault": true
          }
        ]
      }
    ],
    "variants": [
      {
        "publicId": "variant_d57409e1-770b-4f01-9a1e-34858e600517",
        "sku": "103A20030801E04",
        "productId": "926ce61a-811a-4c1d-b985-0f3a03a30538",
        "name": "Salad Test",
        "price": 100,
        "subscriptionPrice": 100,
        "mrp": 100,
        "currency": "INR",
        "weight": 1050,
        "weightUnit": "gm",
        "length": 100,
        "lengthUnit": "mm",
        "breadth": 100,
        "breadthUnit": "mm",
        "height": 100,
        "heightUnit": "mm",
        "availableInStock": 100,
        "stockLimit": 100,
        "inStock": true,
        "isPublished": true,
        "isDefault": true,
        "createdAt": "2026-08-31T07:35:02.509Z",
        "updatedAt": "2026-08-31T14:44:25.634Z",
        "options": [
          {
            "option": "option_weight_26877b3788d24bd89e9b1643400c6cf8",
            "optionValue": "value_1kg_42533bc335f94d06903a2179119e439d"
          }
        ]
      }
    ],
    "recipes": [],
    "presentInWishlist": false,
    "website": "https://redotterfarms.health/products/salad-test"
  }
}
```

### Not Found Error Response

```json
{
  "success": false,
  "message": "product not found"
}
```

---

## Update Variant

Update variant information using its SKU.

### Endpoint

```http
PATCH /webhook/v2/variant/{sku}
```

### Full URL

```text
/webhook/v2/variant/{sku}
```

### Headers

```http
Content-Type: application/json
x-api-key: YOUR_API_KEY
```

### Path Parameters

| Parameter | Type     | Required | Description                  |
| --------- | -------- | -------- | ---------------------------- |
| `sku`     | `string` | Yes      | SKU of the variant to update |

### Request Body

```json
{
  "price": <number>,
  "mrp": <number>,
  "subscription_price": <number>,
  "stock_limit": <number>,
  "is_published": <boolean>,
  "in_stock": <boolean>,
  "available_in_stock": <number>
}
```

### Example Request

```http
PATCH /webhook/v2/variant/{sku}
Content-Type: application/json
x-api-key: YOUR_API_KEY
```

```json
{
  "price": 1,
  "mrp": 1,
  "subscription_price": 1,
  "stock_limit": 100,
  "is_published": true,
  "in_stock": true,
  "available_in_stock": 100
}
```

### Example Success Response

```json
{
  "success": true,
  "message": "variant updated"
}
```

### Example Validation Error Response

```json
{
  "errors": [
    {
      "field": "price",
      "message": "Price is required and must be a number",
      "code": "invalid_type"
    },
    {
      "field": "mrp",
      "message": "MRP is required and must be a number",
      "code": "invalid_type"
    },
    {
      "field": "subscription_price",
      "message": "Subscription price is required and must be a number",
      "code": "invalid_type"
    },
    {
      "field": "stock_limit",
      "message": "Stock limit is required and must be a number",
      "code": "invalid_type"
    },
    {
      "field": "is_published",
      "message": "is_published is required and must be a boolean",
      "code": "invalid_type"
    },
    {
      "field": "in_stock",
      "message": "in_stock is required and must be a boolean",
      "code": "invalid_type"
    },
    {
      "field": "available_in_stock",
      "message": "available_in_stock is required and must be a number",
      "code": "invalid_type"
    }
  ],
  "success": false
}
```
