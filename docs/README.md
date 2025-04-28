# Xash API Documentation

Welcome to the Xash API documentation. This guide will help you integrate with our platform to access a wide range of services including airtime purchases, data bundles, electricity tokens, WiFi vouchers, and money transfers.

## Overview

The Xash API provides a RESTful interface for developers to integrate payment and vending services into their applications. Our API allows you to:

- Purchase airtime and data bundles
- Buy electricity tokens
- Generate WiFi vouchers
- Transfer funds between users
- Generate transaction reports

## Base URL

All API requests should be made to the following base URL:

```
https://api.xash.co.zw/v1
```

## Authentication

The Xash API uses token-based authentication. All API requests must include an `Authorization` header with a valid Bearer token.

See the [Authentication](authentication.md) section for more details on how to obtain and use API tokens.

## Response Format

All API responses are returned in JSON format. A typical successful response will have the following structure:

```json
{
  "success": true,
  "data": {
    // Response data specific to the endpoint
  }
}
```

Error responses will have the following structure:

```json
{
  "success": false,
  "error": {
    "code": "error_code",
    "message": "Human-readable error message",
    "details": {
      // Additional error details (optional)
    }
  }
}
```

## Rate Limiting

API requests are subject to rate limiting to ensure fair usage and system stability. Current rate limits are:

- 60 requests per minute per API token
- 1000 requests per day per API token

If you exceed these limits, you will receive a `429 Too Many Requests` response.

## Support

If you need assistance with the API, please contact our developer support team at:

- Email: developers@xash.co.zw
- Support Portal: https://support.xash.co.zw
