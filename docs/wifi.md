# WiFi (Equal Vouchers)

The WiFi API allows you to query available WiFi voucher options (provided by "Equal") and purchase them based on their specific characteristics.

## Endpoints

### Get Available WiFi Voucher Options

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/wifi/available-options</div>

Retrieves the distinct types of currently available WiFi voucher options based on their denomination (price), data limit, and duration.

**Headers:**

| Name          | Value                   | Required |
|---------------|-------------------------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes      |
| Accept        | application/json        | Yes      |

**Example Response:**
```json
{
  "success": true,
  "message": "Available Equal voucher values",
  "data": [
    {
      "denomination": "1.00",
      "duration": 1,
      "data_limit": "500 MB",
      "duration_in": "hours"
    },
    {
      "denomination": "2.00",
      "duration": 1,
      "data_limit": "1 GB",
      "duration_in": "hours"
    },
    {
      "denomination": "5.00",
      "duration": 24,
      "data_limit": "Unlimited",
      "duration_in": "hours"
    }
    // ... other available options
  ]
}
```

### Buy WiFi Voucher

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/wifi/vouchers/buy</div>

Purchases one or more WiFi vouchers of a specific type.

**Headers:**

| Name           | Value                   | Required |
|----------------|-------------------------|----------|
| Authorization  | Bearer YOUR_ACCESS_TOKEN | Yes      |
| Content-Type   | application/json        | Yes      |
| Accept         | application/json        | Yes      |

**Request Parameters:**

| Name          | Type    | Required | Description                                           | Example     |
|---------------|---------|----------|-------------------------------------------------------|-------------|
| `data_limit`  | string  | Yes      | The data limit of the voucher (e.g., "500 MB", "1 GB"). Must match an available option. | "500 MB"    |
| `duration`    | integer | Yes      | The duration value (e.g., 1, 24). Must match an available option. | 1           |
| `duration_in` | string  | Yes      | The unit for duration (e.g., "hours", "days"). Must match an available option. | "hours"     |
| `amount`      | number  | Yes      | The price (denomination) of a *single* voucher. Must match an available option. | 1.00        |
| `currency`    | string  | Yes      | The currency code. Currently only "USD" is supported. | "USD"       |
| `quantity`    | integer | Yes      | Number of vouchers of this specific type to purchase. Must be >= 1. | 2           |

**Example Request:**
```json
{
  "data_limit": "500 MB",
  "duration": 1,
  "duration_in": "hours",
  "amount": 1.00,
  "currency": "USD",
  "quantity": 2
}
```

**Example Success Response (Reflecting `BuyEqualVoucherResource`):**
```json
{
  "success": true,
  "message": "Equal WiFi Voucher purchase was successful",
  "data": {
    "status": "success",
    "type": "EQUAL_VOUCHER", // Example type from transaction meta
    "id": "a1b2c3d4-e5f6-7890-1234-567890abcdef", // Transaction UUID
    "reference": "XREF-ABCDEF123", // Example derived reference (format may vary)
    "name": "Equal WiFi Voucher", // From transaction meta
    "amount": "2.00", // Total amount charged (formatted string)
    "voucher_value": "1.00", // Single voucher value (formatted string)
    "currency": "USD",
    "balance": { // Structure from BalanceResource (example)
        "currency": "USD",
        "available": "998.00", // Example remaining balance
        "formatted": "USD 998.00"
    },
    "vouchers": [ // Array of actual purchased voucher details from transaction meta
      {
        "pin": "1122334455",
        "serial": "EQSN00001",
        "denomination": "1.00", // Example details associated with the purchased voucher
        "data_limit": "500 MB",
        "duration": 1,
        "duration_in": "hours"
        // Other fields depend on what EqualVoucher::buy() returns and stores
      },
      {
        "pin": "6677889900",
        "serial": "EQSN00002",
        "denomination": "1.00",
        "data_limit": "500 MB",
        "duration": 1,
        "duration_in": "hours"
      }
    ],
    "commission": "USD0.10", // Formatted commission earned (from transaction meta)
    "receipt_footer": "Get airtime/bundle on your change from as little as 10 cents.", // System setting
    "created_at": "2023-10-27T10:00:00.000000Z", // Transaction creation timestamp
    "recharge_instructions": "1. Connect to \"Equal WiFi.\"\n2. Click \"Sign In\" to access the network.\n3. Enter your voucher and press \"Connect\" or visit eql.co.zw"
  }
}
```

**Example Validation Error Response (e.g., Insufficient Stock):**
```json
{
    "message": "The given data was invalid.",
    "errors": {
        "quantity": [
            "The requested quantity exceeds the available stock."
        ]
    }
}
```

**Example Error Response (Insufficient Funds):**
```json
{
    "success": false,
    "message": "You do not have enough balance to make this transaction.",
    "errors": [] // May be empty or contain other details depending on implementation
}
```

## Best Practices

1.  **Query Available Options**: Always call the `GET /api/v1/wifi/available-options` endpoint first to get the exact `data_limit`, `duration`, `duration_in`, and `denomination` (`amount`) combinations currently available for purchase.
2.  **Use Exact Values**: When calling the `POST /api/v1/wifi/buy` endpoint, use the precise string/numeric values returned by the `available-options` endpoint for `data_limit`, `duration`, `duration_in`, and `amount`. Do not guess or construct these values.
3.  **Check Stock**: The API validates if enough vouchers of the *exact specified type* are available before processing the purchase. Be prepared to handle the "quantity exceeds available stock" validation error (HTTP 422).
4.  **Verify Transaction Status**: Check the `success` flag in the response to confirm the transaction was accepted. Examine the `data.status` field if available for more detail.
5.  **Retrieve Voucher Details**: The purchased voucher details (PINs/Serials) are located within the `data.vouchers` array in the success response. Securely store and display these to the end-user.

## Error Handling

Common WiFi voucher purchase errors include:

| Error Message Snippet                     | HTTP Status | Description                                                                   | Resolution                                                                                                  |
|-------------------------------------------|-------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| `data_limit` / `duration` / etc. `exists` | 422         | The specified voucher characteristic combination does not exist or is invalid | Use values obtained directly from the `GET /api/v1/wifi/available-options` endpoint.                        |
| `quantity` exceeds available stock        | 422         | Not enough vouchers of the specified type are available                       | Reduce the `quantity` or try purchasing a different voucher type. Query `available-options` again if needed. |
| `amount` / `quantity` invalid             | 422         | Amount/quantity format/value rules violated (e.g., negative, non-integer)     | Ensure `amount` is positive numeric and `quantity` is a positive integer >= 1.                              |
| Insufficient funds (`message` field)      | 400         | The user's wallet balance is insufficient for the total purchase amount       | Add funds to the user's wallet or reduce the `quantity` / choose a cheaper voucher.                         |
| Service unavailable / Internal Error      | 500 / 503   | Internal server error or temporary issue processing the request               | Try the request again later. If the issue persists, contact support.                                        |
| Unauthorized                              | 401         | Invalid, missing, or expired access token                                     | Ensure a valid `Authorization: Bearer YOUR_ACCESS_TOKEN` header is sent. Refresh the token if necessary.    |