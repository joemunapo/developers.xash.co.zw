# Error Handling

This guide explains how to handle errors that may occur when using the Xash API.

## Error Response Format

When an error occurs, the API returns an appropriate HTTP status code and a JSON response with error details:

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

## HTTP Status Codes

The Xash API uses standard HTTP status codes to indicate the success or failure of requests:

| Status Code | Description |
|-------------|-------------|
| 200 | OK - The request was successful |
| 400 | Bad Request - The request was malformed or contained invalid parameters |
| 401 | Unauthorized - Authentication is required or the provided credentials are invalid |
| 403 | Forbidden - The authenticated user does not have permission to access the requested resource |
| 404 | Not Found - The requested resource was not found |
| 422 | Validation Failed - The request failed validation checks |
| 429 | Too Many Requests - Rate limit exceeded |
| 500 | Server Error - An unexpected error occurred on the server |

## Common Error Codes

| Error Code | Description | Typical Resolution |
|------------|-------------|-------------------|
| `bad_request` | The request was malformed | Check your request format and parameters |
| `unauthorized` | Authentication is required | Ensure you're including a valid access token |
| `forbidden` | Permission denied | Verify your account has access to the requested resource |
| `not_found` | Resource not found | Check that the resource ID or endpoint is correct |
| `validation_failed` | Input validation failed | Check the error details for specific validation errors |
| `insufficient_funds` | Insufficient wallet balance | Add funds to your wallet or reduce the transaction amount |
| `invalid_phone` | Invalid phone number format | Ensure the phone number is in the correct format (e.g., 263712345678) |
| `service_unavailable` | Service temporarily unavailable | Retry the request after a short delay |
| `rate_limit_exceeded` | Rate limit exceeded | Reduce request frequency or wait until rate limit resets |
| `server_error` | Unexpected server error | Contact support if the error persists |

## Validation Errors

For validation errors (HTTP 422), the response includes details about which fields failed validation:

```json
{
  "success": false,
  "error": {
    "code": "validation_failed",
    "message": "The given data was invalid",
    "details": {
      "phone": ["The phone field is required"],
      "amount": ["The amount must be a positive number"]
    }
  }
}
```

## Handling Errors in Your Application

Here are some best practices for handling API errors in your application:

1. **Check the success flag**: Always check the `success` field in the response to determine if the request was successful.

2. **Handle common errors gracefully**: Display user-friendly error messages for common errors.

3. **Implement retry logic**: For transient errors (like rate limiting or temporary service unavailability), implement retry logic with exponential backoff.

4. **Log detailed errors**: Log the complete error response for debugging purposes.

5. **Validate input before sending**: Validate user input on the client side before sending requests to reduce validation errors.

## Example Error Handling

Here's an example of how to handle errors in JavaScript:

```javascript
async function makeApiRequest(endpoint, method, data) {
  try {
    const response = await fetch(`https://dev.xash.co.zw/api/v1/${endpoint}`, {
      method: method,
      headers: {
        'Authorization': `Bearer ${accessToken}`,
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      body: data ? JSON.stringify(data) : undefined
    });

    const result = await response.json();

    if (!result.success) {
      // Handle error based on error code
      switch (result.error.code) {
        case 'unauthorized':
          // Refresh token or redirect to login
          refreshToken();
          break;
        case 'insufficient_funds':
          // Show user-friendly message
          showError('Insufficient funds in your wallet. Please add funds and try again.');
          break;
        case 'validation_failed':
          // Show validation errors
          showValidationErrors(result.error.details);
          break;
        case 'rate_limit_exceeded':
          // Implement retry with backoff
          await sleep(2000);
          return makeApiRequest(endpoint, method, data);
        default:
          // Generic error handling
          showError(`Error: ${result.error.message}`);
          console.error('API Error:', result.error);
      }
      return null;
    }

    return result.data;
  } catch (error) {
    // Network or parsing error
    console.error('Request failed:', error);
    showError('Network error. Please check your connection and try again.');
    return null;
  }
}
```

## Contacting Support

If you encounter persistent errors or need help understanding an error, contact our support team:

- Email: developers@xash.co.zw
- Support Portal: https://support.xash.co.zw

When contacting support about an error, please include:
- The full error response
- The request you were trying to make
- Any steps to reproduce the error
- Your client ID (never share your client secret)
