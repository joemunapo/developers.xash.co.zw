# Electricity

The Electricity API allows you to validate meter accounts and purchase electricity tokens.

## Endpoints

### Check Account

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/electricity/check-account</div>

Validates an electricity meter account before purchasing tokens.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| meter_number | string | Yes | Electricity meter number |

**Example Request:**
```json
{
  "meter_number": "12345678901"
}
```

**Example Response:**
```json
{
  "success": true,
  "data": {
    "meter_number": "12345678901",
    "customer_name": "John Doe",
    "address": "123 Main Street, Harare",
    "meter_type": "Prepaid",
    "is_valid": true
  }
}
```

### Buy Tokens

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/electricity/buy-tokens</div>

Purchases electricity tokens for a validated meter.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| meter_number | string | Yes | Electricity meter number |
| amount | number | Yes | Amount to purchase (in USD) |

**Example Request:**
```json
{
  "meter_number": "12345678901",
  "amount": 10
}
```

**Example Response:**
```json
{
  "success": true,
  "data": {
    "transaction_id": "tx_123456",
    "token": "1234-5678-9012-3456-7890",
    "units": "52.7 kWh",
    "amount": 10,
    "currency": "USD",
    "customer_name": "John Doe",
    "meter_number": "12345678901"
  }
}
```

## Best Practices

1. **Always validate meter numbers**: Use the check-account endpoint to validate meter numbers before purchasing tokens.

2. **Keep token records**: Store electricity tokens securely as they cannot be retrieved once lost.

3. **Verify transaction status**: Always check the transaction status in the response to ensure the purchase was successful.

4. **Check minimum amounts**: Different electricity providers may have different minimum purchase amounts.

5. **Handle customer information**: Display customer information from the check-account response to the user for verification before purchase.

## Error Handling

Common electricity-related errors include:

| Error | Description | Resolution |
|-------|-------------|------------|
| Invalid meter number | The meter number format is incorrect or not found | Verify the meter number and try again |
| Meter validation failed | The meter could not be validated | Check the meter number or contact support |
| Minimum amount not met | The purchase amount is below the minimum required | Increase the purchase amount |
| Insufficient funds | The wallet balance is insufficient for the purchase | Add funds to your wallet or reduce the purchase amount |
| Service unavailable | The electricity provider service is temporarily unavailable | Try again later or contact support |
