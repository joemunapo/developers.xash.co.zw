# Getting Started

This guide will help you get started with the Xash API. Follow these steps to begin integrating our services into your application.

## Prerequisites

Before you begin, you'll need:

1. A Xash account with API access
2. Your API credentials (client ID and client secret)
3. Basic knowledge of REST APIs and HTTP requests

## Registration

If you don't have a Xash account yet, you can register at [https://xash.co.zw/register](https://xash.co.zw/register). After registration, contact our support team to request API access for your account.

For detailed information about the registration process, please refer to the [Authentication](authentication.md#register) section which explains how to:
1. [Register a new account](authentication.md#register)
2. [Set your password](authentication.md#set-password)
3. [Login to obtain an access token](authentication.md#login)

## API Keys

Once your account is approved for API access, you can generate your API keys from the developer dashboard. You'll need these keys to authenticate your API requests.

## Making Your First Request

Here's a simple example of how to make your first API request to check your wallet balance:

```bash
curl -X GET \
  https://api.xash.co.zw/v1/wallet \
  -H 'Authorization: Bearer YOUR_API_TOKEN' \
  -H 'Content-Type: application/json'
```

## Next Steps

Now that you understand the basics, you can:

1. Learn more about [Authentication](authentication.md)
2. Understand how to [Make Requests](making-requests.md)
3. Explore the [API Reference](api-reference.md) to see all available endpoints
4. Check out our [SDKs & Libraries](sdks.md) for your preferred programming language

## Development Workflow

A typical development workflow with the Xash API looks like this:

1. Obtain API credentials
2. Implement authentication
3. Test API endpoints in a sandbox environment
4. Implement error handling
5. Move to production

## Environments

Xash provides two environments for API integration:

### Sandbox

```
https://sandbox-api.xash.co.zw/v1
```

Use the sandbox environment for testing your integration without making real transactions. All operations in the sandbox environment use test data and do not affect real accounts or services.

### Production

```
https://api.xash.co.zw/v1
```

Once your integration is tested and ready, you can switch to the production environment to make real transactions.

## API Versioning

The Xash API uses versioning to ensure backward compatibility. The current version is `v1`. When a new version is released, we'll provide migration guides to help you update your integration.
