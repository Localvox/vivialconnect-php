# VivialConnect PHP Client Library

VivialConnect is a simple SMS/MMS API. It's designed specifically for developers seeking a simple, affordable and scalable messaging solution.

Get your API key here: https://www.vivialconnect.net/register <br>
Be sure to read the API documentation: https://www.vivialconnect.net/docs <br>
Library documentation lives here: https://vivialconnect.github.io/vivialconnect-php/namespaces/VivialConnect.html


Requirements
------------

* Any flavour of PHP 5.3.3+
* [Guzzle - PHP HTTP client](https://github.com/guzzle/guzzle)
* [Onion - standalone middleware library](https://github.com/esbenp/onion)

Installation
------------

The installation of Composer is really simple:

```
mkdir bin
curl -s http://getcomposer.org/installer | php -- --install-dir=bin
```

Next, run the Composer command to install the latest stable version of VivialConnect PHP client:

```
php bin/composer.phar require vivialconnect/sdk
```

You can also manually add metadata into your local `composer.json` file, at the top level of the project:

```
{
    "require": {
        "vivialconnect/sdk": "0.2.0"
    }
}
```

And run the composer install command:

```
php bin/composer.phar install
```

In the last step you will include composer autogenerated `autoload.php` file into your project, and you are ready to go:

```php

require 'vendor/autoload.php';

```

You can then later update VivialConnect PHP client using composer:

```
php bin/composer.phar update
```

Examples
--------

```php

require __DIR__ . '/vendor/autoload.php';

use VivialConnect\Resources\Message;
use VivialConnect\Resources\Resource;

Resource::setCredentialToken(Resource::API_KEY, "my-api-key");
Resource::setCredentialToken(Resource::API_SECRET, "my-api-secret");
Resource::setCredentialToken(Resource::API_ACCOUNT_ID, "12345678");
Resource::init();

function sendMessage($body, $fromNumber, $toNumber)
{
    $message = new Message;
    $message->body = $body;
    $message->from_number = $fromNumber;
    $message->to_number = $toNumber;
    $message->send();
}

sendMessage('Howdy, from Vivial Connect!',
            '+10982599999', '+11234561111');
```

```php

require __DIR__ . '/vendor/autoload.php';

use VivialConnect\Resources\Number;
use VivialConnect\Resources\Resource;

Resource::setCredentialToken(Resource::API_KEY, "my-api-key");
Resource::setCredentialToken(Resource::API_SECRET, "my-api-secret");
Resource::setCredentialToken(Resource::API_ACCOUNT_ID, "12345678");
Resource::init();

function buyNumber($name, $phoneNumber,
                   $areaCode, $phoneNumberType = 'local')
{
    $number = new Number;
    $number->name = $name;
    $number->phone_number = $phoneNumber;
    $number->area_code = $areaCode;
    $number->phone_number_type = $phoneNumberType;
    $number->buy();
}

buyNumber('(123) 259-7591', '+11232597591', '123');

```

```php

require __DIR__ . '/vendor/autoload.php';

use VivialConnect\Resources\Number;
use VivialConnect\Resources\Resource;

Resource::setCredentialToken(Resource::API_KEY, "my-api-key");
Resource::setCredentialToken(Resource::API_SECRET, "my-api-secret");
Resource::setCredentialToken(Resource::API_ACCOUNT_ID, "12345678");
Resource::init();

function listAvailableNumbers($countryCode = 'US', $phoneNumberType = 'local',
                              $areaCode = '913', $page = 1, $limit = 20)
{
    $qs = ['page' => $page, 'limit' => $limit, 'area_code' => $areaCode];
    $numbers = Number::searchAvailable($countryCode, $phoneNumberType, $qs);
    foreach ($numbers as $key => $number)
    {
        printf("name = %s\n", $number->name);
        printf("phone_number = %s\n", $number->phone_number);
        printf("phone_number_type = %s\n", $number->phone_number_type);
        print("\n");
    }
}

listAvailableNumbers();
```
