# AstimPay Laravel SDK

![AstimPay Logo](https://astimpay.com/assets/images/logo.png)

The AstimPay Laravel SDK allows you to seamlessly integrate the AstimPay payment gateway into your Laravel applications.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
  - [Initializing the SDK](#initializing-the-sdk)
  - [Initializing a Payment](#initializing-a-payment)
  - [Verifying a Payment](#verifying-a-payment)
  - [Handling IPN Notifications](#handling-ipn-notifications-optional)
- [Notes](#notes)

## Installation
Run the following command in your project directory:

```bash
composer require astimpay/laravel-sdk
```
## Usage

### Initializing the SDK


```php
use AstimPay\LaravelSDK\AstimPay;
```

### Initializing a Payment

To initiate a payment, follow these steps:

1. Initialize the `AstimPay` class with your API key and base URL:

```php
$apiKey = "4a59f023cbc02521417f21a0add4e028febb2ca8"; // API KEY
$apiBaseURL = "https://sandbox.astimpay.com/api/checkout-v1"; // API URL
$astimpay = new AstimPay($apiKey, $apiBaseURL);
```

2. Prepare payment request data and initiate payment:

```php
// Example request data for initializing a payment
$requestData = [
    'full_name'     => "John Doe",
    'email'         => "test@test.com",
    'amount'        => 100,
    'metadata'      => [
        'example_metadata_key' => "example_metadata_value",
        // ... Add more key-value pairs for dynamic metadata ...
    ],
    'redirect_url'  => 'http://localhost/success.php', // add your success route
    'return_type'   => 'GET',
    'cancel_url'    => 'http://localhost/cancel.php', // add your cancel route
    'webhook_url'   => 'http://localhost/ipn.php', // add your ipn route
];

try {
    $paymentUrl = $astimpay->initPayment($requestData);
    return redirect($paymentUrl);
} catch (\Exception $e) {
     dd("Initialization Error: " . $e->getMessage());
}
```


#### Available API Types

The `initPayment` method allows you to specify the API type as the second parameter. The available options are:

- `checkout-v1`: Advanced checkout API (default, Success Page notification only).



### Verifying a Payment

To verify a payment, follow these steps:

1. Initialize the `AstimPay` class as shown in the previous steps.

2. Get the invoice ID from the payment success page:

```php
$invoiceId = $request->invoice_id;
```

3. Verify the payment:

```php
try {
    $response = $astimpay->verifyPayment($invoiceId);
    dd($response); // Display the verification response
} catch (\Exception $e) {
    dd("Verification Error: " . $e->getMessage());
}
```

### Handling IPN Notifications (Optional)

To handle IPN (Instant Payment Notification) requests, follow these steps:

1. Initialize the `AstimPay` class as shown in the previous steps.

2. Use the `executePayment` method:

```php
try {
    $ipnResponse = $astimpay->executePayment();
    dd($ipnResponse);
} catch (\Exception $e) {
     dd("Error: " . $e->getMessage());
}
```



## Notes

- Replace `"API KEY"` with your actual API key.
- Adjust the request data and other details according to your project requirements.
- The `metadata` field is dynamic; you can add multiple key-value pairs as needed.
- Make sure to handle errors using try-catch blocks as demonstrated above.