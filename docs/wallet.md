# Wallet

The Wallet API allows you to manage your Xash wallet, check balances, and view transaction history.

## Endpoints

### Get Wallet Information

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/wallet</div>

Retrieves wallet information for all currencies associated with the authenticated user.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
    "success": true,
    "message": "Wallet retrieved successfully",
    "commission_release_date": "2025-05-05T04:30:00+02:00",
    "data": [
        {
            "currency": "USD",
            "name": "US Dollar",
            "profit_on_hold": "1.629",
            "balance": "57.40"
        },
        {
            "currency": "ZiG",
            "name": "Zimbabwe Gold",
            "profit_on_hold": "0.000",
            "balance": "0.00"
        }
    ]
}
```

### Get Wallet by Currency

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/wallet/{currency}</div>

Retrieves wallet information for a specific currency.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | Yes | Currency code (e.g., USD, ZWL) |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "success": true,
  "data": {
    "currency": "USD",
    "balance": 100.50
  }
}
```

## Payment Methods

### InnBucks Payment

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/innbucks/pay</div>

Initiates an InnBucks payment to add funds to your wallet.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| amount | number | Yes | Payment amount |
| phone | string | Yes | InnBucks account phone number |

**Example Request:**
```json
{
  "amount": 50,
  "phone": "263712345678"
}
```

**Example Response:**
```json
{
  "success": true,
  "data": {
    "payment_id": "innbucks_12345",
    "poll_url": "/api/v1/innbucks/poll/innbucks_12345"
  }
}
```

### Poll InnBucks Payment

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/innbucks/poll/{payment}</div>

Checks the status of an InnBucks payment.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| payment | string | Yes | InnBucks payment ID |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "success": true,
  "data": {
    "status": "completed",
    "message": "Payment completed successfully"
  }
}
```

### EcoCash Payment

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/ecocash/pay</div>

Initiates an EcoCash payment to add funds to your wallet.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| amount | number | Yes | Payment amount |
| phone | string | Yes | EcoCash account phone number |

**Example Request:**
```json
{
  "amount": 50,
  "phone": "263712345678"
}
```

**Example Response:**
```json
{
  "success": true,
  "data": {
    "payment_id": "ecocash_12345",
    "poll_url": "/api/v1/ecocash/poll/ecocash_12345"
  }
}
```

### Poll EcoCash Payment

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/ecocash/poll/{payment}</div>

Checks the status of an EcoCash payment.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| payment | string | Yes | EcoCash payment ID |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "success": true,
  "data": {
    "status": "completed",
    "message": "Payment completed successfully"
  }
}
```

## Best Practices

1. **Check balances before transactions**: Always verify that you have sufficient funds before initiating a transaction.

2. **Poll payment status**: When making payments, poll the status endpoint until you receive a completed or failed status.

3. **Handle currency correctly**: Be explicit about which currency you're using for transactions.

4. **Monitor transaction history**: Regularly check your transaction history to reconcile your account.

## Error Handling

Common wallet-related errors include:

| Error | Description | Resolution |
|-------|-------------|------------|
| Insufficient funds | The wallet balance is insufficient for the requested transaction | Add funds to your wallet or reduce the transaction amount |
| Invalid currency | The specified currency is not supported | Use a supported currency (e.g., USD, ZWL) |
| Payment failed | The payment process failed | Check the payment details and try again |
| Payment pending | The payment is still being processed | Continue polling the payment status endpoint |
