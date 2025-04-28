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
| currency | string | Yes | Set this to USD |
| amount | string | Yes | set this amount to 5 |

**Example Request:**
```json
{
  {
  "currency" :"USD",
  "meter_number" : "123456789",
  "amount":"5"
  }
}
```

**Example Response:**
```json
{
    "success": true,
    "message": "Account verified successfully.",
    "data": {
        "customer_name": "John Doe",
        "customer_address": "1st Street, Address",
        "meter_number": "123456789",
        "meter_currency": "ZWG",
        "success": true
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
  {
  "currency" :"USD",
  "meter_number" : "123456789",
  "amount":"5"
  }
}
```

**Example Response:**
```json
{
    "success": true,
    "message": "ZESA tokens purchased successfully.",
    "data": {
        "customer_name": "John Doe",
        "customer_address": "1st Street, Address",
        "meter_number": "123456789",
        "meter_currency": "ZWG",
        "success": true,
        "reference": "74A033C39C1D",
        "kwh": "29.52",
        "energy": "ZWG64.95",
        "debt": "ZWG0.00",
        "rea": "ZWG3.90",
        "vat": "ZWG0.00",
        "tendered_currency": "USD",
        "tendered": "USD5.00",
        "total_amt": "ZWG68.85",
        "date": "28/04/25 11:40",
        "tokens": [
            {
                "token": "56961115316491527310",
                "units": "29.52",
                "formatted": "5696 1115 3164 9152 7310",
                "rate": "29.52@2.2: ",
                "receipt": "00001250428114032443",
                "tax_rate": "0.00",
                "net_amount": "64.95",
                "tax_amount": "0.00",
                "position": 1
            }
        ],
        "id": "9ec858f4-e514-4ab6-aa0d-74a033c39c1d",
        "name": "Electricity purchase",
        "type": "electricity",
        "amount": "5",
        "currency": "USD",
        "balance": {
            "currency": "USD",
            "name": "US Dollar",
            "profit_on_hold": "5.582",
            "balance": "9.20"
        },
        "commission": "USD0.125",
        "receipt_footer": "Thanks!",
        "created_at": "2025-04-28T09:40:30.000000Z"
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
