# Webhooks

The Xash API supports webhooks to notify your application of events that happen in your Xash account. This allows you to automate actions in response to these events.

## Overview

Webhooks are HTTP callbacks that are triggered when specific events occur in your Xash account. When an event occurs, Xash sends an HTTP POST request to the URL you've configured, containing information about the event.

## Setting Up Webhooks

To set up webhooks for your account, contact our support team at developers@xash.co.zw with the following information:

1. The URL where you want to receive webhook notifications
2. The events you want to subscribe to
3. An optional secret key for verifying webhook signatures

## Webhook Events

The following events are available for webhook notifications:

| Event | Description |
|-------|-------------|
| `transaction.completed` | A transaction has been completed |
| `transaction.failed` | A transaction has failed |
| `wallet.credited` | Funds have been added to your wallet |
| `wallet.debited` | Funds have been deducted from your wallet |
| `transfer.initiated` | A transfer has been initiated |
| `transfer.completed` | A transfer has been completed |
| `transfer.failed` | A transfer has failed |

## Webhook Payload

Webhook notifications are sent as HTTP POST requests with a JSON payload. The payload includes information about the event and relevant data.

Example payload for a `transaction.completed` event:

```json
{
  "event": "transaction.completed",
  "timestamp": "2025-04-09T08:15:00Z",
  "data": {
    "transaction_id": "tx_123456",
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
    }
  }
}
```

## Verifying Webhook Signatures

To ensure that webhook notifications are coming from Xash, we include a signature in the `X-Xash-Signature` header of each request. You can verify this signature using your webhook secret.

The signature is created by computing an HMAC-SHA256 hash of the request body using your webhook secret as the key.

Example verification in PHP:

```php
$payload = file_get_contents('php://input');
$signature = $_SERVER['HTTP_X_XASH_SIGNATURE'];
$secret = 'your_webhook_secret';

$computedSignature = hash_hmac('sha256', $payload, $secret);

if (hash_equals($computedSignature, $signature)) {
    // Signature is valid, process the webhook
    $event = json_decode($payload, true);
    // Process the event
} else {
    // Signature is invalid, reject the webhook
    http_response_code(403);
    echo json_encode(['error' => 'Invalid signature']);
}
```

## Best Practices

1. **Respond quickly**: Your webhook endpoint should respond with a 200 status code as quickly as possible. Process the webhook asynchronously if needed.

2. **Implement retry logic**: If your endpoint is temporarily unavailable, Xash will retry the webhook delivery with exponential backoff.

3. **Verify signatures**: Always verify webhook signatures to ensure the requests are coming from Xash.

4. **Handle duplicate events**: Implement idempotency to handle duplicate webhook deliveries, which may occur during retries.

5. **Monitor webhook failures**: Set up monitoring for webhook failures to detect and resolve issues quickly.

6. **Use HTTPS**: Ensure your webhook endpoint uses HTTPS to secure the data in transit.

7. **Log webhook events**: Keep logs of webhook events for debugging and auditing purposes.

## Troubleshooting

If you're not receiving webhook notifications, check the following:

1. Ensure your webhook URL is publicly accessible
2. Verify that your server is correctly configured to accept HTTP POST requests
3. Check that your webhook URL is correctly registered with Xash
4. Verify that you're subscribed to the events you're expecting to receive
5. Check your server logs for any errors in processing webhook requests

If you continue to experience issues with webhooks, contact our support team at developers@xash.co.zw.
