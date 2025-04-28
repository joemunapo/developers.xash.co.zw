# Transfers

The Transfers API allows you to transfer funds between Xash users.

## Endpoints

### Initiate Transfer

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/transfer</div>

Initiates a transfer to another user.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| recipient | string | Yes | Recipient user number or phone |
| amount | number | Yes | Amount to transfer |
| currency | string | Yes | Currency code (e.g., USD, ZWL) |
| note | string | No | Optional note for the transfer |

**Example Request:**
```json
{
  "recipient": "USR12345",
  "amount": 25.50,
  "currency": "USD",
  "note": "Payment for services"
}
```

**Example Response:**
```json
{
  "success": true,
  "data": {
    "transfer_id": "tr_123456",
    "status": "pending",
    "confirmation_url": "/api/v1/transfer/confirm/tr_123456",
    "message": "Transfer initiated. Please confirm to complete."
  }
}
```

### Confirm Transfer

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/transfer/confirm/{transfer}</div>

Confirms a pending transfer.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| transfer | string | Yes | Transfer ID |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "success": true,
  "data": {
    "transfer_id": "tr_123456",
    "status": "completed",
    "transaction_id": "tx_123456",
    "recipient": "USR12345",
    "amount": 25.50,
    "currency": "USD",
    "message": "Transfer completed successfully"
  }
}
```

## Best Practices

1. **Verify recipient**: Always double-check the recipient user number or phone before initiating a transfer.

2. **Confirm transfers**: Transfers require explicit confirmation to complete. Always call the confirm endpoint after initiating a transfer.

3. **Include descriptive notes**: Add clear notes to transfers to help recipients understand the purpose of the transfer.

4. **Check sufficient funds**: Ensure you have sufficient funds in your wallet before initiating a transfer.

5. **Verify transaction status**: Always check the transaction status in the response to ensure the transfer was successful.

## Error Handling

Common transfer-related errors include:

| Error | Description | Resolution |
|-------|-------------|------------|
| Recipient not found | The specified recipient does not exist | Check the recipient user number or phone |
| Insufficient funds | The wallet balance is insufficient for the transfer | Add funds to your wallet or reduce the transfer amount |
| Invalid currency | The specified currency is not supported | Use a supported currency (e.g., USD, ZWL) |
| Transfer not found | The specified transfer ID does not exist | Check the transfer ID and try again |
| Transfer already confirmed | The transfer has already been confirmed | No action needed, the transfer is already complete |
| Transfer expired | The transfer confirmation window has expired | Initiate a new transfer |
