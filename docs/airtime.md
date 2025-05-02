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
    "message": "Supported carriers",
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "Econet",
            "commission": "9.00",
            "has_direct_airtime": true,
            "has_voucher": true,
            "ussd_code": "*440*112#",
            "vendor_ussd_code": "*121*PIN#",
            "has_bundle": true,
            "has_vendor_voucher": true,
            "vendor_voucher_commission": "5.00"
        },
        {
            "id": 2,
            "name": "Telecel",
            "commission": "5.00",
            "has_direct_airtime": true,
            "has_voucher": false,
            "ussd_code": "",
            "vendor_ussd_code": "",
            "has_bundle": false,
            "has_vendor_voucher": false,
            "vendor_voucher_commission": "0.00"
        },
        {
            "id": 3,
            "name": "Netone",
            "commission": "7.00",
            "has_direct_airtime": true,
            "has_voucher": true,
            "ussd_code": "*133*PIN#",
            "vendor_ussd_code": "*133*PIN#",
            "has_bundle": false,
            "has_vendor_voucher": false,
            "vendor_voucher_commission": "7.00"
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
| currency | string | Yes | Currency eg USD or ZWL |

**Example Request:**
```json
{
  "mobile_phone": "263712345678",
  "amount": 5,
  "carrier_id": 1,
  "currency":"USD"
}
```

**Example Response:**
```json
{
    "success": true,
    "message": "Recharge to +263712345678 was successful",
    "data": {
        "name": "Netone airtime",
        "type": "direct_airtime",
        "id": "9ec84a6c-11b6-4ff5-8f8a-dacbb57e2e88",
        "reference": "DACBB57E2E88",
        "amount": "1",
        "currency": "USD",
        "mobile_phone": "+263712345678",
        "status": "success",
        "balance": {
            "currency": "USD",
            "name": "US Dollar",
            "profit_on_hold": "1.699",
            "balance": "56.40"
        },
        "commission": "USD0.07",
        "receipt_footer": "Thanks!",
        "created_at": "2025-04-28T08:59:52.000000Z"
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
    "message": "Econet airtime values",
    "allow_custom_amount": true,
    "data": [
        "0.50",
        "1.00",
        "2.00"
    ],
    "vendor_voucher_values": [
        "5.00",
        "10.00"
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
| quantity | integer | Yes | Number of vouchers to purchase |
| is_vendor_voucher | Boolean | Yes | Check to see if is vendor voucher |
| currency | string | Yes | currency eg USD, ZWL |

**Example Request:**
```json
{
  "quantity": "1",
  "amount": "0.5",
  "carrier_id": 3,
  "currency":"USD",
  "is_vendor_voucher": false
}
```

**Example Response:**
```json
{
    "success": true,
    "message": "Netone xash voucher purchase was successful",
    "data": {
        "status": "success",
        "type": "voucher",
        "id": "9ec8536e-56ea-4090-98e7-fcca2252c3ed",
        "reference": "FCCA2252C3ED",
        "name": "Netone xash voucher",
        "amount": "1.00",
        "voucher_value": "1.00",
        "currency": "USD",
        "balance": {
            "currency": "USD",
            "name": "US Dollar",
            "profit_on_hold": "1.839",
            "balance": "54.40"
        },
        "vouchers": [
            "5304 6082 2442"
        ],
        "commission": "USD0.07",
        "receipt_footer": "Thanks!",
        "created_at": "2025-04-28T09:25:03.000000Z",
        "recharge_instructions": "*133*PIN#"
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
