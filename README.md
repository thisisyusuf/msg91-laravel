# Laravel service provider for Msg91

_This library requires a minimum PHP version of 7.1_

This is a **[laravel](https://laravel.com) service provider** for [Msg91 APIs](https://docs.msg91.com/collection/msg91-api-integration/5/pages/139). It wraps the [msg91-php][client] client and provides the same functionality for Laravel applications by exposing a Service Provider and Facade.

> **NOTE**: The project is under active development and so, some apis are subjected to change before of `v1.0.0` release.

## Table of Contents

-   [Installation](#installation)
-   [Configuration](#configuration)
-   [Usage](#usage)
-   [Examples](#examples)
    -   [Create a Client](#create-a-client)
    -   [Managing OTPs](#managing-otps)
        -   [Send OTP](#send-otp)
        -   [Verify OTP](#verify-otp)
        -   [Resend OTP](#resend-otp)

## Installation

The packages is available on [Packagist](https://packagist.org/packages/craftsys/msg91-laravel) and can be installed via [Composer](https://getcomposer.org/) by executing following command in shell.

```bash
composer require craftsys/msg91-laravel
```

### Laravel 5.5+

If you're using Laravel 5.5 or above, the package will automatically register the `Craftsys\Msg91\Msg91LaravelServiceProvider` provider and aliases `Craftsys\Msg91\Facade\Msg91` facade to `Msg91`.

### Laravel 5.4 and below

Add `Craftsys\Msg91\Msg91LaravelServiceProvider` to the `providers` array in your `config/app.php`:

```php
'providers' => [
	 // Other service providers...
	 Craftsys\Msg91\Msg91LaravelServiceProvider::class,
],
```

If you want to use the facade interface, you can `use` the facade class when needed:

```php
use Craftsys\Msg91\Facade\Msg91;
```

Or add an alias in your `config/app.php`

```php
'aliases' => [
	// other aliases here
	'Msg91' => Craftsys\Msg91\Facade\Msg91::class,
],
```

To verify that everything is working as expected, excecute the following php code somewhere in your application, either
in an example route or in `php artisan tinker` if you are in Laravel.

```php
// this should print the `\Craftsys\Msg91\Services\OTPService` of some default configuration values
echo Msg91::otp()::class
```

If there is an issue, please check the steps again or open an issue for support.

## Configuration

As the [msg91-php][client] offers configuration that are similar to Laravel's configuration, this package simply ports the Laravel's configuration to the msg91-php client.

The package can be configured by providing a `msg91` key inside your `config/services.php` configuration file.

```php
<?php

return [
	// along with other services
	"msg91": [
		'key' => env("Msg91_KEY"),
	],
];
```

and update the `.env` file to get the desired values e.g. `Msg91_KEY`.

Please visit [msg91-php configuration][client-configuration] for a detailed description about the available options and their default values.

## Usage

Once you have [Configured](#configuration) the Laravel/Lumen application to use the service provider and have aliased the facade to `Msg91`, you will have to [msg91-php][client] client `Craftsys\Msg91\Client`'s instance.

```php
Msg91::otp()
	->to(919999999999)
	->send()
```

Next, follow along with [examples](#examples) to learn more

## Examples

### Managing OTPs

OTP services like sending, verifying, and resending etc, can be accessed via `otp` method on the client instance e.g. `Msg91::otp()`.

> For a detailed usage, please visit [msg91-php's documentation][client-managing-otps] on managing OTPs.

#### Send OTP

```php
Msg91::otp()
	->to(912343434312) // phone number with country code
	->send(); // send the otp
```

### Verify OTP

```php
Msg91::otp(1234) // OTP to be verified
	->to(912343434312) // phone number with country code
	->verify(); // Verify
```

### Resend OTP

```php
Msg91::otp()
	->to(912343434312) // set the mobile with country code
	->via("text") // way of retry
	->resend(); // resend otp
```

> For all the examples and options, please consult [msg91-php examples section][client-examples]

[client]: https://github.com/craftsys/msg91-php
[client-configuration]: https://github.com/craftsys/msg91-php#configuration
[client-examples]: https://github.com/craftsys/msg91-php#examples
[client-managing-otps]: https://github.com/craftsys/msg91-php#managing-otps
