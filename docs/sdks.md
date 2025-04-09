# SDKs & Libraries

Xash provides official SDKs and libraries to help you integrate with our API more easily. This page provides information about available SDKs and how to use them.

## Available SDKs

### PHP SDK

Our official PHP SDK makes it easy to integrate Xash API into your PHP applications.

#### Installation

Install the SDK using Composer:

```bash
composer require xash/php-sdk
```

#### Basic Usage

```php
<?php
require 'vendor/autoload.php';

use Xash\XashClient;

// Initialize the client with your API credentials
$client = new XashClient([
    'api_key' => 'YOUR_API_KEY',
    'environment' => 'production' // or 'sandbox' for testing
]);

// Get wallet balance
try {
    $response = $client->wallet->getBalance('USD');
    echo "Your USD balance: " . $response->data->balance;
} catch (\Xash\Exception\ApiException $e) {
    echo "Error: " . $e->getMessage();
}

// Purchase airtime
try {
    $response = $client->airtime->buyDirect([
        'phone' => '263712345678',
        'amount' => 5,
        'carrier_id' => 1
    ]);
    echo "Airtime purchase successful. Transaction ID: " . $response->data->transaction_id;
} catch (\Xash\Exception\ApiException $e) {
    echo "Error: " . $e->getMessage();
}
```

### JavaScript SDK

Our JavaScript SDK is designed for both browser and Node.js environments.

#### Installation

Install the SDK using npm:

```bash
npm install xash-js-sdk
```

Or using yarn:

```bash
yarn add xash-js-sdk
```

#### Basic Usage

```javascript
import { XashClient } from 'xash-js-sdk';

// Initialize the client with your API credentials
const client = new XashClient({
  apiKey: 'YOUR_API_KEY',
  environment: 'production' // or 'sandbox' for testing
});

// Get wallet balance
client.wallet.getBalance('USD')
  .then(response => {
    console.log(`Your USD balance: ${response.data.balance}`);
  })
  .catch(error => {
    console.error('Error:', error.message);
  });

// Purchase airtime
client.airtime.buyDirect({
  phone: '263712345678',
  amount: 5,
  carrier_id: 1
})
  .then(response => {
    console.log(`Airtime purchase successful. Transaction ID: ${response.data.transaction_id}`);
  })
  .catch(error => {
    console.error('Error:', error.message);
  });
```

## Community Libraries

In addition to our official SDKs, there are community-maintained libraries for various programming languages:

### Python

```bash
pip install xash-python
```

### Java

```bash
mvn install com.xash:xash-java-sdk:1.0.0
```

### .NET

```bash
dotnet add package Xash.NET
```

## API Reference Documentation

For detailed information about the API endpoints and parameters, refer to the [API Reference](api-reference.md) section.

## Contributing

If you'd like to contribute to our SDKs or develop a library for a language we don't currently support, please contact us at developers@xash.co.zw.

## Support

If you need help with our SDKs or libraries, you can:

1. Check the documentation for your specific SDK
2. Open an issue on the SDK's GitHub repository
3. Contact our developer support team at developers@xash.co.zw
