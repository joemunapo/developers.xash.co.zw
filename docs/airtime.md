# Airtime

The Airtime API allows you to purchase airtime directly for phone numbers or as vouchers for various mobile carriers.

## Endpoints

### Get Airtime Carriers

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/airtime/carriers</div>

Retrieves a list of available airtime carriers.

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
      "name": "Econet",
      "code": "econet"
    },
    {
      "id": 2,
      "name": "NetOne",
      "code": "netone"
    }
  ]
}
```

### Buy Direct Airtime

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/airtime/direct</div>

Purchases airtime directly for a phone number.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| phone | string | Yes | Recipient phone number |
| amount | number | Yes | Airtime amount |
| carrier_id | integer | Yes | ID of the carrier |

**Example Request:**
```json
{
  "phone": "263712345678",
  "amount": 5,
  "carrier_id": 1
}
```

**Example Response:**
```json
{
  "success": true,
  "data": {
    "transaction_id": "tx_123456",
    "status": "completed",
    "message": "Airtime purchased successfully"
  }
}
```

### Get Voucher Values

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/airtime/voucher/{carrier}/values</div>

Retrieves available voucher values for a specific carrier.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| carrier | string | Yes | Carrier code or ID |

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
      "value": 5,
      "currency": "USD"
    },
    {
      "id": 2,
      "value": 10,
      "currency": "USD"
    }
  ]
}
```

### Buy Airtime Voucher

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/airtime/voucher/{carrier}/buy</div>

Purchases an airtime voucher for a specific carrier.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| carrier | string | Yes | Carrier code or ID |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| value_id | integer | Yes | ID of the voucher value |
| quantity | integer | No | Number of vouchers to purchase (default: 1) |

**Example Request:**
```json
{
  "value_id": 1,
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
        "value": 5,
        "currency": "USD",
        "carrier": "Econet"
      },
      {
        "code": "2345-6789-0123-4567",
        "value": 5,
        "currency": "USD",
        "carrier": "Econet"
      }
    ]
  }
}
```

## Best Practices

1. **Verify carrier support**: Always check the list of supported carriers before attempting to purchase airtime.

2. **Validate phone numbers**: Ensure phone numbers are in the correct format for the carrier (e.g., 263712345678).

3. **Check voucher values**: Use the voucher values endpoint to get the list of available denominations before purchasing vouchers.

4. **Store voucher codes securely**: When purchasing vouchers, store the codes securely until they are used.

5. **Verify transaction status**: Always check the transaction status in the response to ensure the purchase was successful.

## Error Handling

Common airtime-related errors include:

| Error | Description | Resolution |
|-------|-------------|------------|
| Invalid carrier | The specified carrier is not supported | Use a supported carrier from the carriers endpoint |
| Invalid phone number | The phone number format is incorrect | Ensure the phone number is in the correct format for the carrier |
| Invalid voucher value | The requested voucher value is not available | Use a value from the voucher values endpoint |
| Insufficient funds | The wallet balance is insufficient for the purchase | Add funds to your wallet or reduce the purchase amount |
| Service unavailable | The carrier service is temporarily unavailable | Try again later or contact support |
