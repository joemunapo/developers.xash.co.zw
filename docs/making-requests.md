# Making Requests

This guide explains how to make requests to the Xash API, including request formats, headers, and best practices.

## Request Format

All API requests should be made using HTTPS to the base URL:

```
https://dev.xash.co.zw/api/v1

```

## HTTP Methods

The Xash API uses standard HTTP methods:

- **GET**: Retrieve resources
- **POST**: Create resources or perform actions
- **PUT**: Update resources (not commonly used in this API)
- **DELETE**: Delete resources (not commonly used in this API)

## Headers

Include the following headers with all API requests:

| Header | Value | Description |
|--------|-------|-------------|
| `Authorization` | `Bearer YOUR_ACCESS_TOKEN` | Your API access token |
| `Content-Type` | `application/json` | Format of the request body |
| `Accept` | `application/json` | Format of the response |

Example:

```bash
curl -X GET \
  https://dev.xash.co.zw/api/v1/wallet \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'
```

## Request Body

For POST and PUT requests, include a JSON-formatted request body:

```bash
curl -X POST \
  https://dev.xash.co.zw/api/v1/airtime/direct \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
  -H 'Content-Type: application/json' \
  -d '{
    "phone": "263712345678",
    "amount": 5,
    "carrier_id": 1
  }'
```

## Query Parameters

For GET requests, you can include query parameters in the URL:

```bash
curl -X GET \
  'https://dev.xash.co.zw/api/v1/reports/mini/USD/week' \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

## Pagination

Some endpoints that return lists of items support pagination. You can control pagination using the following query parameters:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `page` | Page number | 1 |
| `per_page` | Number of items per page | 15 |

Example:

```bash
curl -X GET \
  'https://dev.xash.co.zw/api/v1/reports/history/USD?page=2&per_page=20' \
  -H 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Paginated responses include pagination metadata:

```json
{
  "success": true,
  "data": {
    "transactions": [
      // Transaction data
    ]
  },
  "meta": {
    "current_page": 2,
    "per_page": 20,
    "total": 156,
    "total_pages": 8
  }
}
```

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

## Error Handling

If an error occurs, the API will return an appropriate HTTP status code and a JSON response with error details:

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

See the [Error Handling](error-handling.md) section for more information on error codes and how to handle them.

## Request Limits

- Maximum request body size: 10MB
- Maximum file upload size: 5MB
- Rate limits: 60 requests per minute, 1000 requests per day

## Best Practices

1. **Validate input data** before sending it to the API
2. **Handle errors gracefully** in your application
3. **Implement retry logic** with exponential backoff for failed requests
4. **Cache responses** when appropriate to reduce API calls
5. **Use HTTPS** for all requests to ensure data security
6. **Monitor your API usage** to stay within rate limits
7. **Include all required parameters** to avoid validation errors
8. **Check response status** before processing the response data
9. **Log API interactions** for debugging and auditing purposes
10. **Keep your access tokens secure** and never expose them in client-side code
