# FAQs

This page provides answers to frequently asked questions about the Xash API.

## General Questions

### What is the Xash API?

The Xash API is a RESTful interface that allows developers to integrate Xash services into their applications. These services include airtime purchases, data bundles, electricity tokens, WiFi vouchers, and money transfers.

### What can I do with the Xash API?

With the Xash API, you can:
- Purchase airtime and data bundles
- Buy electricity tokens
- Generate WiFi vouchers
- Transfer funds between users
- Generate transaction reports

### Is there a sandbox environment for testing?

Yes, Xash provides a sandbox environment for testing your integration without making real transactions. The sandbox environment is available at:

```
https://sandbox-api.xash.co.zw/v1
```

### How do I get access to the API?

To get access to the Xash API, you need to:
1. Register for a Xash account at [https://xash.co.zw/register](https://xash.co.zw/register)
2. Contact our support team to request API access for your account
3. Once approved, you can generate your API credentials from the developer dashboard

## Authentication

### How does authentication work?

The Xash API uses token-based authentication. You need to register an account, set a password, and then login to obtain an access token. This token must be included in the Authorization header of all API requests.

### How long are access tokens valid?

Access tokens do not expire automatically but can be invalidated by:
- Logging out using the logout endpoint
- Changing the user's password
- Administrator action

### What should I do if my token is compromised?

If you believe your access token has been compromised, you should:
1. Immediately log out using the logout endpoint to invalidate the token
2. Change your password
3. Contact our support team at developers@xash.co.zw

## Transactions

### How do I check the status of a transaction?

You can check the status of a transaction using the transaction details endpoint:

```
GET /api/v1/reports/transaction/{transaction}
```

### What currencies are supported?

The Xash API currently supports the following currencies:
- USD (US Dollar)
- ZWL (Zimbabwean Dollar)

### Are there any transaction limits?

Yes, there are transaction limits that vary by service and user account type. Contact our support team for specific limits for your account.

### How do refunds work?

If a transaction fails after your wallet has been debited, the funds will be automatically refunded to your wallet. If you need to request a manual refund, please contact our support team with the transaction ID.

## Technical Questions

### What is the rate limit for API requests?

The current rate limits are:
- 60 requests per minute per API token
- 1000 requests per day per API token

### How should I handle API errors?

All API errors return a standard error format with a success flag set to false, an error code, and a human-readable message. You should check the success flag in all responses and handle errors appropriately in your application.

### Do you provide webhooks for event notifications?

Yes, Xash supports webhooks to notify your application of events that happen in your account. See the [Webhooks](webhooks.md) section for more information.

### Are there SDKs available for different programming languages?

Yes, Xash provides official SDKs for PHP and JavaScript, as well as community-maintained libraries for Python, Java, and .NET. See the [SDKs & Libraries](sdks.md) section for more information.

## Support

### How do I get help with API integration?

If you need assistance with API integration, you can:
1. Refer to this documentation
2. Contact our developer support team at developers@xash.co.zw
3. Visit our support portal at https://support.xash.co.zw

### How do I report a bug or suggest a feature?

To report a bug or suggest a feature, please contact our developer support team at developers@xash.co.zw with detailed information about the issue or suggestion.

### Is there a community forum for developers?

We're currently working on setting up a community forum for developers. In the meantime, you can join our Telegram group for developer discussions. Contact our support team for an invitation link.
