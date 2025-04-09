# Reports

The Reports API allows you to retrieve transaction history, mini statements, and commission reports for your Xash account.

## Endpoints

### Mini Statements

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/reports/mini/{currency}/{range}</div>

Retrieves mini statements for a specific currency and time range.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | Yes | Currency code (e.g., USD, ZWL) |
| range | string | Yes | Time range (today, week, month) |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "success": true,
  "data": {
    "total_in": 150.00,
    "total_out": 75.50,
    "balance": 74.50,
    "currency": "USD",
    "transactions": [
      {
        "id": "tx_123456",
        "type": "credit",
        "amount": 100.00,
        "description": "Wallet deposit",
        "date": "2025-04-09T07:30:00Z"
      },
      {
        "id": "tx_123457",
        "type": "debit",
        "amount": 25.50,
        "description": "Airtime purchase",
        "date": "2025-04-09T08:15:00Z"
      }
    ]
  }
}
```

### Account History

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/reports/history/{currency}</div>

Retrieves account history for a specific currency.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | Yes | Currency code (e.g., USD, ZWL) |

**Query Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| per_page | integer | No | Number of items per page (default: 15) |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "success": true,
  "data": {
    "transactions": [
      {
        "id": "tx_123456",
        "type": "credit",
        "amount": 100.00,
        "currency": "USD",
        "description": "Wallet deposit",
        "reference": "ref_123456",
        "status": "completed",
        "date": "2025-04-09T07:30:00Z"
      },
      {
        "id": "tx_123457",
        "type": "debit",
        "amount": 25.50,
        "currency": "USD",
        "description": "Airtime purchase",
        "reference": "ref_123457",
        "status": "completed",
        "date": "2025-04-09T08:15:00Z"
      }
    ]
  },
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 50,
    "total_pages": 4
  }
}
```

### Commission Reports

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/reports/commission/{currency}/{range?}</div>

Retrieves commission reports for a specific currency and optional time range.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | Yes | Currency code (e.g., USD, ZWL) |
| range | string | No | Time range (today, week, month) |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "success": true,
  "data": {
    "total_commission": 15.25,
    "currency": "USD",
    "range": "month",
    "commissions": [
      {
        "id": "cm_123456",
        "amount": 5.75,
        "service": "Airtime",
        "transaction_id": "tx_123456",
        "date": "2025-04-05T10:30:00Z"
      },
      {
        "id": "cm_123457",
        "amount": 9.50,
        "service": "Electricity",
        "transaction_id": "tx_123457",
        "date": "2025-04-08T14:45:00Z"
      }
    ]
  }
}
```

### Transaction Details

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/reports/transaction/{transaction}</div>

Retrieves details for a specific transaction.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| transaction | string | Yes | Transaction ID |

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "success": true,
  "data": {
    "id": "tx_123456",
    "type": "debit",
    "amount": 25.50,
    "currency": "USD",
    "description": "Airtime purchase",
    "reference": "ref_123456",
    "status": "completed",
    "service": "Airtime",
    "meta": {
      "phone": "263712345678",
      "carrier": "Econet"
    },
    "date": "2025-04-09T08:15:00Z"
  }
}
```

## Best Practices

1. **Use pagination**: When retrieving account history, use pagination to manage large datasets efficiently.

2. **Filter by currency**: Always specify the currency when retrieving reports to get accurate financial information.

3. **Choose appropriate time ranges**: Select the most appropriate time range for your reporting needs.

4. **Store transaction IDs**: Keep track of transaction IDs for future reference and detailed lookups.

5. **Regular reconciliation**: Regularly check your transaction history to reconcile your account.

## Error Handling

Common report-related errors include:

| Error | Description | Resolution |
|-------|-------------|------------|
| Invalid currency | The specified currency is not supported | Use a supported currency (e.g., USD, ZWL) |
| Invalid range | The specified time range is not supported | Use a supported range (today, week, month) |
| Transaction not found | The specified transaction ID does not exist | Check the transaction ID and try again |
| Pagination limit exceeded | The requested page is beyond the available data | Adjust the page number to be within range |
| No data available | There are no transactions for the specified criteria | Try different filter criteria or check account activity |
