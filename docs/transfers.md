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
    "message": "Transfer initiated successfully.",
    "data": {
        "id": "9ec85ae2-22e8-4e7c-b64b-335e3ce0b97f",
        "recipient": "2789714848",
        "first_name": "John",
        "last_name": "Doe",
        "amount": "1",
        "currency": "USD",
        "reference": null
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
    "message": "Transfer completed successfully.",
    "data": {
        "name": "Transfer to Dev Tech O - Assign Test 1001",
        "type": "transfer",
        "id": "9ec85bbf-e1d4-49e1-a00c-497c45285ae7",
        "reference": null,
        "balance": {
            "currency": "USD",
            "name": "US Dollar",
            "profit_on_hold": "5.582",
            "balance": "8.20"
        },
        "transaction_id": "9ec85bbf-e1d4-49e1-a00c-497c45285ae7",
        "direction": "out",
        "recipient": "2789716061",
        "amount": "-1",
        "currency": "USD",
        "first_name": "John",
        "last_name": "Doe",
        "created_at": "2025-04-28T09:48:19.000000Z"
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
