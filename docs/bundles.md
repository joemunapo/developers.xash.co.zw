# Data Bundles

The Data Bundles API allows you to purchase mobile data bundles directly for phone numbers or as vouchers for various mobile carriers.

## Endpoints

### Get Available Bundles

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/bundles</div>

Retrieves a list of available data bundles across all carriers.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
    "success": true,
    "message": "Available bundles",
    "data": [
        {
            "id": 1,
            "name": "$0.20 = Voice Daily USD Bundle",
            "price": "0.2",
            "description": "",
            "network": "Econet",
            "valid_for": "1",
            "currency": "USD",
            "type": "Data"
        },
        {
            "id": 2,
            "name": "$0.30 300Mb + 8 SMS",
            "price": "0.3",
            "description": "$0.30 300Mb + 8 SMS",
            "network": "Econet",
            "valid_for": "1",
            "currency": "USD",
            "type": "Data"
        },
   	.....

    ]
}
```

### Buy Data Bundle Directly

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/bundles/buy/{bundle}</div>

Purchases a data bundle directly for a phone number.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| bundle | integer | Yes | Bundle ID |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| phone | string | Yes | Recipient phone number |

**Example Request:**
```json
{
  "mobile_phone": "263712345678"
}
```

**Example Response:**
```json
{
    "message": "$0.20 = Voice Daily USD Bundle bundle purchased successfully",
    "success": true,
    "data": {
        "name": "$0.20 = Voice Daily USD Bundle",
        "type": "direct_bundle",
        "id": "9ec85596-0cdd-4f1d-aab2-4f5ede51d7fb",
        "reference": "4F5EDE51D7FB",
        "amount": "0.2",
        "currency": "USD",
        "mobile_phone": "0772345678",
        "status": "success",
        "balance": {
            "currency": "USD",
            "name": "US Dollar",
            "profit_on_hold": "1.857",
            "balance": "54.20"
        },
        "commission": "USD0.018",
        "receipt_footer": "Thanks!",
        "created_at": "2025-04-28T09:31:05.000000Z"
    }
}
```

### Buy Data Bundle Voucher

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/bundles/voucher/buy/{bundle}</div>

Purchases a data bundle voucher.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| bundle | integer | Yes | Bundle ID |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| quantity | integer | No | Number of vouchers to purchase (default: 1) |

**Example Request:**
```json
{
  "quantity": 2
}
```

**Example Response:**
```json
{
    "message": "$0.20 = Voice Daily USD Bundle bundle purchased successfully",
    "success": true,
    "data": {
        "name": "$0.20 = Voice Daily USD Bundle",
        "type": "direct_bundle",
        "id": "9ec85596-0cdd-4f1d-aab2-4f5ede51d7fb",
        "reference": "4F5EDE51D7FB",
        "amount": "0.2",
        "currency": "USD",
        "mobile_phone": "0772345678",
        "status": "success",
        "balance": {
            "currency": "USD",
            "name": "US Dollar",
            "profit_on_hold": "1.857",
            "balance": "54.20"
        },
        "commission": "USD0.018",
        "receipt_footer": "Thanks!",
        "created_at": "2025-04-28T09:31:05.000000Z"
    }
}
```

## Best Practices

1. **Check bundle availability**: Always verify that the bundle is available and active before attempting to purchase.

2. **Validate phone numbers**: Ensure phone numbers are in the correct format for the carrier (e.g., 263712345678).

3. **Verify carrier compatibility**: Make sure the recipient's phone number is compatible with the carrier of the bundle.

4. **Store voucher codes securely**: When purchasing vouchers, store the codes securely until they are used.

5. **Verify transaction status**: Always check the transaction status in the response to ensure the purchase was successful.

## Error Handling

Common data bundle-related errors include:

| Error | Description | Resolution |
|-------|-------------|------------|
| Bundle not found | The specified bundle ID does not exist | Check the bundle ID and try again |
| Invalid phone number | The phone number format is incorrect | Ensure the phone number is in the correct format for the carrier |
| Bundle not active | The requested bundle is not currently active | Choose an active bundle from the bundles endpoint |
| Insufficient funds | The wallet balance is insufficient for the purchase | Add funds to your wallet or choose a less expensive bundle |
| Service unavailable | The carrier service is temporarily unavailable | Try again later or contact support |
