# API Reference

This section provides a comprehensive reference for all available endpoints in the Xash API. Each endpoint is documented with its HTTP method, URL, required parameters, and response format.

## Available Endpoints

The Xash API is organized into the following categories:

- [Wallet](#wallet): Manage your wallet balance and transactions
- [Airtime](#airtime): Purchase airtime directly or as vouchers
- [Data Bundles](#data-bundles): Purchase data bundles for mobile devices
- [Electricity](#electricity): Purchase electricity tokens
- [WiFi](#wifi): Purchase WiFi vouchers
- [Transfers](#transfers): Transfer funds between users
- [Reports](#reports): Generate transaction reports and statements

For detailed information about each category, please refer to their respective documentation pages.

## Response Format

All API responses follow a standard format:

```json
{
  "success": true,
  "data": {
    // Response data specific to the endpoint
  }
}
```

For error responses, see the [Error Handling](error-handling.md) documentation.

## Pagination

Endpoints that return lists of items support pagination using the following query parameters:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `page` | Page number | 1 |
| `per_page` | Number of items per page | 15 |

Paginated responses include pagination metadata:

```json
{
  "success": true,
  "data": {
    // Response data
  },
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 50,
    "total_pages": 4
  }
}
```

## Rate Limits

All API endpoints are subject to rate limiting:

- 60 requests per minute per API token
- 1000 requests per day per API token

If you exceed these limits, you will receive a `429 Too Many Requests` response.
