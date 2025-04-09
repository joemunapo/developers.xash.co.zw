# WiFi

The WiFi API allows you to purchase WiFi vouchers for various service providers.

## Endpoints

### Get WiFi Vouchers

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/wifi/vouchers</div>

Retrieves available WiFi voucher options.

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
      "name": "1 Hour Access",
      "description": "1 hour of high-speed WiFi access",
      "price": 1.00,
      "currency": "USD",
      "provider": "ZOL",
      "is_active": true
    },
    {
      "id": 2,
      "name": "24 Hour Access",
      "description": "24 hours of high-speed WiFi access",
      "price": 5.00,
      "currency": "USD",
      "provider": "ZOL",
      "is_active": true
    }
  ]
}
```

### Buy WiFi Voucher

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/wifi/vouchers/buy</div>

Purchases a WiFi voucher.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| voucher_id | integer | Yes | ID of the voucher option |
| quantity | integer | No | Number of vouchers to purchase (default: 1) |

**Example Request:**
```json
{
  "voucher_id": 1,
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
        "code": "WIFI-1234-5678",
        "pin": "9012",
        "name": "1 Hour Access",
        "provider": "ZOL",
        "price": 1.00,
        "currency": "USD",
        "expires_at": "2025-04-16T08:12:43Z"
      },
      {
        "code": "WIFI-2345-6789",
        "pin": "0123",
        "name": "1 Hour Access",
        "provider": "ZOL",
        "price": 1.00,
        "currency": "USD",
        "expires_at": "2025-04-16T08:12:43Z"
      }
    ]
  }
}
```

## Best Practices

1. **Check voucher availability**: Always verify that the voucher option is available and active before attempting to purchase.

2. **Store voucher details securely**: WiFi vouchers typically include a code and PIN that should be stored securely until used.

3. **Note expiration dates**: WiFi vouchers usually have an expiration date after which they cannot be used.

4. **Verify transaction status**: Always check the transaction status in the response to ensure the purchase was successful.

5. **Consider bulk purchases**: If you need multiple vouchers, use the quantity parameter to purchase them in a single transaction.

## Error Handling

Common WiFi-related errors include:

| Error | Description | Resolution |
|-------|-------------|------------|
| Voucher not found | The specified voucher ID does not exist | Check the voucher ID and try again |
| Voucher not active | The requested voucher is not currently active | Choose an active voucher from the vouchers endpoint |
| Insufficient funds | The wallet balance is insufficient for the purchase | Add funds to your wallet or reduce the quantity |
| Service unavailable | The WiFi provider service is temporarily unavailable | Try again later or contact support |
| Maximum quantity exceeded | The requested quantity exceeds the maximum allowed | Reduce the quantity or make multiple purchases |
