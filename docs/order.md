# Standard Order vs Subscription Order

This document describes the payload structure for **standard orders** and **subscription orders**, including the fields used to identify the order and the corresponding Razorpay payment.

The majority of the payload structure is shared between both order types. The primary difference is how the order is associated with its source:

- **Standard Order** → identified using `checkout_id`
- **Subscription Order** → identified using `subscription_id`, `cycle_id`, and `cycle_no`

---

# Standard Order

A standard order is created from a normal customer checkout.

## Order Identification

The standard order uses:

```json
{
  "checkout_id": "checkout_..."
}
```

`checkout_id` identifies the checkout from which the order was created.

The same value is also sent to Razorpay as:

```text
checkoutId
```

in the Razorpay payment notes.

## Standard Order Payload

```json
{
  "id": "order_1fdba767-7857-4d41-b123-6bc1aae70f8e",
  "checkout_id": "checkout_1fdba767-7857-4d41-b123-6bc1aae70f8e",
  "order_date": "2026-09-16",
  "customer_id": "96965000041126003",
  "mobile": "+917827676141",
  "payment_status": "PAID",
  "billing_address": {
    "address_id": "96965000041126009",
    "fax": "",
    "zip": "110018",
    "city": "Tilak Nagar",
    "phone": "+917827676141",
    "state": "Delhi",
    "email": "damanjeetsingh434@gmail.com",
    "county": "",
    "address": "WZ-26",
    "country": "India",
    "street2": "Street-19, Sant Garh",
    "state_code": "DL",
    "country_code": "IN"
  },
  "shipping_address": {
    "address_id": "96965000041126011",
    "fax": "",
    "zip": "110018",
    "city": "Tilak Nagar",
    "phone": "+917827676141",
    "state": "Delhi",
    "email": "damanjeetsingh434@gmail.com",
    "county": "",
    "address": "WZ-26",
    "country": "India",
    "street2": "Street-19, Sant Garh",
    "state_code": "DL",
    "country_code": "IN"
  },
  "order_items": {
    "items": [
      {
        "Name": "Box of Twenty",
        "Price": 6076,
        "Currency": "INR",
        "Quantity": 1,
        "ProductRetailerId": "101A34419701D60",
        "SKU": "101A34419701D60",
        "subscription_start_date": "na",
        "subscription_frequency": "na",
        "subscription_length": "na",
        "renewal_orderno": "na"
      }
    ]
  },
  "payment_object": {
    "cart_value": 6076,
    "otterwallet": 3675,
    "rof_campaign": {
      "coupon": ""
    },
    "split_payment": 0,
    "delivery_charge": 0,
    "discount_amount": 304,
    "customer_payment": {
      "useRazorpay": true,
      "useOtterWallet": false,
      "total_payment_due": 5772,
      "razorpay_payment_amount": 5772,
      "otterwallet_payment_amount": 0
    },
    "total_order_value": 5772
  },
  "razorpay_payment": {
    "amount": 5772,
    "date": "2026-09-16",
    "reference_number": "pay_TcoFgUL33wJIx0",
    "bank_charges": 148,
    "description": {
      "RazorPay Reference ID": "pay_TcoFgUL33wJIx0",
      "RazorPay Payment Method": "netbanking",
      "RazorPay Customer Phone ": "+917827676141"
    },
    "account_id": "96965000001672139"
  },
  "source": "Website - NCR",
  "place_of_supply": "DL"
}
```

## Standard Order → Razorpay

The relationship is:

```text
checkout_id
     ↓
Order
     ↓
Razorpay Payment
```

The `checkout_id` is sent to Razorpay as:

```text
checkoutId
```

This allows the Razorpay payment to be matched against the corresponding internal order.

---

# Subscription Order

A subscription order is generated for a specific billing cycle of an existing subscription.

Instead of using `checkout_id`, a subscription order contains three subscription-specific identifiers:

```json
{
  "subscription_id": "subscription_...",
  "cycle_id": "subscription_attempt_...",
  "cycle_no": 1
}
```

## Subscription Order Identifiers

### `subscription_id`

Identifies the parent subscription.

```text
subscription_id
```

### `cycle_id`

Identifies the specific subscription payment attempt/cycle.

```text
cycle_id
```

### `cycle_no`

Identifies the sequential subscription cycle number.

```text
cycle_no
```

For example:

```text
Cycle 1 → cycle_no: 1
Cycle 2 → cycle_no: 2
Cycle 3 → cycle_no: 3
```

## Subscription Order Payload

```json
{
  "id": "order_94d4afc6-63be-409b-b4d6-be6434b2057a",
  "mobile": "+918766247447",
  "source": "Website - NCR",
  "cycle_id": "subscription_attempt_29122439-0165-4c06-91d7-b80f14f66c58",
  "cycle_no": 1,
  "order_date": "2026-09-23",
  "customer_id": "96965000035389898",
  "order_items": {
    "items": [
      {
        "SKU": "103A20030801E04",
        "Name": "test-salad",
        "Price": 1,
        "Currency": "INR",
        "Quantity": 1,
        "ProductRetailerId": "103A20030801E04"
      }
    ]
  },
  "payment_object": {
    "cart_value": 1,
    "otterwallet": 0,
    "rof_campaign": {
      "coupon": ""
    },
    "split_payment": 0,
    "delivery_charge": 0,
    "discount_amount": 0,
    "customer_payment": {
      "useRazorpay": true,
      "useOtterWallet": false,
      "total_payment_due": 1,
      "razorpay_payment_amount": 1,
      "otterwallet_payment_amount": 0
    },
    "total_order_value": 1
  },
  "payment_status": "PAID",
  "billing_address": {
    "fax": "",
    "zip": "201301",
    "city": "Noida",
    "email": "dev@makeamine.com",
    "phone": "+918766247447",
    "state": "Uttar Pradesh",
    "county": "",
    "address": "noida",
    "country": "India",
    "street2": "noida",
    "address_id": "96965000035389900",
    "state_code": "UP",
    "country_code": "IN"
  },
  "place_of_supply": "DL",
  "subscription_id": "subscription_77173bfc-d8d4-4b96-9875-fcbca9c06130",
  "razorpay_payment": {
    "date": "2026-09-23",
    "amount": 1,
    "account_id": "96965000001672139",
    "description": {
      "RazorPay Reference ID": "pay_TfPPMkfL0LDaw9",
      "RazorPay Payment Method": "upi",
      "RazorPay Customer Phone ": "+918766247447"
    },
    "bank_charges": 2,
    "reference_number": "pay_TfPPMkfL0LDaw9"
  },
  "shipping_address": {
    "fax": "",
    "zip": "110049",
    "city": "New Delhi",
    "email": "dev@makeamine.com",
    "phone": "+918766247447",
    "state": "Delhi",
    "county": "",
    "address": "Red Otter Farms",
    "country": "India",
    "street2": "Team",
    "address_id": "96965000035389902",
    "state_code": "DL",
    "country_code": "IN"
  }
}
```

## Subscription Order → Razorpay

The relationship is:

```text
subscription_id
       ↓
cycle_id + cycle_no
       ↓
Subscription Order
       ↓
Razorpay Payment
```

The three subscription identifiers are also sent to Razorpay as payment notes.

Razorpay uses the camelCase versions:

```text
subscriptionId
cycleId
cycleNo
```

This allows the payment to be matched to the correct subscription and, more importantly, to the correct billing cycle.

---

# Key Differences

The following fields are the primary difference between the two order types.

| Purpose                | Standard Order | Subscription Order                     |
| ---------------------- | -------------- | -------------------------------------- |
| Order source           | `checkout_id`  | `subscription_id`                      |
| Specific payment cycle | —              | `cycle_id`                             |
| Cycle number           | —              | `cycle_no`                             |
| Razorpay reference     | `checkoutId`   | `subscriptionId`, `cycleId`, `cycleNo` |

### Standard

```text
checkout_id
     ↓
Order
     ↓
Razorpay Payment
```

### Subscription

```text
subscription_id
       ↓
cycle_id + cycle_no
       ↓
Order
       ↓
Razorpay Payment
```

---

# Razorpay Payment Notes

The values are represented differently between the internal order payload and Razorpay payment notes.

## Standard Order

Internal order payload:

```json
{
  "checkout_id": "checkout_123"
}
```

Razorpay payment notes:

```json
{
  "checkoutId": "checkout_123"
}
```

## Subscription Order

Internal order payload:

```json
{
  "subscription_id": "subscription_123",
  "cycle_id": "subscription_attempt_123",
  "cycle_no": 1
}
```

Razorpay payment notes:

```json
{
  "subscriptionId": "subscription_123",
  "cycleId": "subscription_attempt_123",
  "cycleNo": 1
}
```

> **Important:** The values themselves remain the same. Only the field naming convention changes.
>
> **Order payload:** `snake_case`
> **Razorpay payment notes:** `camelCase`

### Field Mapping

| Order Payload     | Razorpay Payment Notes |
| ----------------- | ---------------------- |
| `checkout_id`     | `checkoutId`           |
| `subscription_id` | `subscriptionId`       |
| `cycle_id`        | `cycleId`              |
| `cycle_no`        | `cycleNo`              |

These references are used to correlate the Razorpay payment with the correct internal order or subscription billing cycle.
