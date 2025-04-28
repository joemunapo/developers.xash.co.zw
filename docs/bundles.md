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
  "data": [
    {
      "id": 1,
      "carrier_id": 1,
      "carrier_name": "Econet",
      "name": "Daily Data 1GB",
      "description": "1GB data valid for 24 hours",
      "code": "econet_daily_1gb",
      "price": 2.50,
      "currency": "USD",
      "is_active": true
    },
    {
      "id": 2,
      "carrier_id": 1,
      "carrier_name": "Econet",
      "name": "Weekly Data 5GB",
      "description": "5GB data valid for 7 days",
      "code": "econet_weekly_5gb",
      "price": 10.00,
      "currency": "USD",
      "is_active": true
    }
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
  "phone": "263712345678"
}
```

**Example Response:**
```json
{
  "success": true,
  "data": {
    "transaction_id": "tx_123456",
    "status": "completed",
    "message": "Data bundle purchased successfully"
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
  "success": true,
  "data": {
    "transaction_id": "tx_123456",
    "vouchers": [
      {
        "code": "1234-5678-9012-3456",
        "bundle_name": "Daily Data 1GB",
        "carrier": "Econet",
        "price": 2.50,
        "currency": "USD"
      },
      {
        "code": "2345-6789-0123-4567",
        "bundle_name": "Daily Data 1GB",
        "carrier": "Econet",
        "price": 2.50,
        "currency": "USD"
      }
    ]
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
