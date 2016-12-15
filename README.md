# Uphold SDK for PHP
[![Latest Version](https://img.shields.io/packagist/v/seegno/uphold-sdk-php.svg)](https://packagist.org/packages/seegno/uphold-sdk-php)
[![Build Status](https://travis-ci.org/seegno/uphold-sdk-php.svg?branch=master)](https://travis-ci.org/seegno/uphold-sdk-php)
[![Code Climate](https://codeclimate.com/github/seegno/uphold-sdk-php/badges/gpa.svg)](https://codeclimate.com/github/seegno/uphold-sdk-php)
[![Test Coverage](https://codeclimate.com/github/seegno/uphold-sdk-php/badges/coverage.svg)](https://codeclimate.com/github/seegno/uphold-sdk-php)
[![License](https://img.shields.io/packagist/l/seegno/uphold-sdk-php.svg)](https://packagist.org/packages/seegno/uphold-sdk-php)

Uphold is a next generation money service business that shields you from bitcoin volatility by enabling you to hold bitcoin as the money you use every day.

The Uphold SDK for PHP provides an easy way for developers to integrate PHP applications with the [Uphold API](https://uphold.com/en/developer/api/documentation).

## Requirements

* PHP >= 5.5 with the [cURL](http://php.net/manual/en/book.curl.php) extension.
* [Guzzle](https://github.com/guzzle/guzzle) library.

## Installation

### Standalone

Grab the latest version of the library:

    $ git clone https://github.com/seegno/uphold-sdk-php.git

Install composer:

    $ curl -s https://getcomposer.org/installer | php

Install the library dependencies:

    $ php composer.phar install

Require the autoloader file generated by composer:

```php
require 'vendor/autoload.php';
```

### Integrating with another project using composer

Require the library package as a dependency:
```json
{
    "require": {
        "seegno/uphold-sdk-php": "~4.0"
    }
}
```

## Basic usage

In order to learn more about the Uphold API, please check out the [API documentation](https://uphold.com/en/developer/api/documentation).

First, create a Personal Access Token (PAT) using the command described below.

### Create a Personal Access Token

    $ php lib/Uphold/console.php tokens:create

Then, create a new instance of the `Client` class with token. Take a look at the following examples and explore more on the [examples](https://github.com/seegno/uphold-sdk-php/tree/master/examples) directory.

### Authorize user
```php
require_once 'vendor/autoload.php';

use Uphold\UpholdClient as Client;

// Initialize the client.
$client = new Client(array(
  'client_id' => 'APPLICATION_CLIENT_ID',
  'client_secret' => 'APPLICATION_CLIENT_SECRET',
));

// Authorize user using the code parameter from the application redirect url.
$user = $client->authorizeUser('CODE');

// Get the Bearer token.
$bearer = $user->getClient()->getOption('bearer');
```

### Get authenticated user
```php
require_once 'vendor/autoload.php';

use Uphold\UpholdClient as Client;

// Initialize the client.
$client = new Client();

// Get user.
$user = $client->getUser('AUTHORIZATION_TOKEN');
```

### Get user balances
```php
require_once 'vendor/autoload.php';

use Uphold\UpholdClient as Client;

// Initialize the client.
$client = new Client();

// Get user.
$user = $client->getUser('AUTHORIZATION_TOKEN');

// Get user balances for all currencies.
$balances = $user->getBalances();
```

You could get user total balance:

```php
// Get user total balance.
$balance = $user->getTotalBalance();
```

The above produces the output shown below:

```php
Array
(
    [amount] => 3.14
    [currency] => BTC
)
```

### Get user cards
```php
require_once 'vendor/autoload.php';

use Uphold\UpholdClient as Client;

// Initialize the client.
$client = new Client();

// Get user.
$user = $client->getUser('AUTHORIZATION_TOKEN');

// Get current user cards.
$cards = $user->getCards();
```

### Create new card
```php
require_once 'vendor/autoload.php';

use Uphold\UpholdClient as Client;

// Initialize the client.
$client = new Client();

// Get the current user.
$user = $client->getUser('AUTHORIZATION_TOKEN');

// Create a new 'BTC' card.
$card = $user->createCard('My new card', 'BTC');
```

The above produces the output shown below:

```php
Uphold\Model\Card Object
(
    [id:protected] => ade869d8-7913-4f67-bb4d-72719f0a2be0
    [address:protected] => Array
        (
            [bitcoin] => 1GpBtJXXa1NdG94cYPGZTc3DfRY2P7EwzH
        )

    [addresses:protected] => Array
        (
            [0] => Array
                (
                    [id] => 1GpBtJXXa1NdG94cYPGZTc3DfRY2P7EwzH
                    [network] => bitcoin
                )

        )

    [available:protected] => 0.00
    [balance:protected] => 0.00
    [currency:protected] => BTC
    [label:protected] => My new card
    [lastTransactionAt:protected] =>
    [transactions:protected] =>
    [settings] => Array
        (
            [position] => 10
        )
)
```

### Get rates
```php
require_once 'vendor/autoload.php';

use Uphold\UpholdClient as Client;

// Initialize the client.
$client = new Client();

// Get rates (public endpoint).
$rates = $client->getRates();
```

Or you could get rates for a specific currency:

```php
// Get rates for BTC.
$rates = $client->getRatesByCurrency('BTC');
```

The above produces the output shown below:

```php
Array
(
    [0] => Uphold\Model\Rate Object
        (
            [ask:protected] => 1
            [bid:protected] => 1
            [currency:protected] => BTC
            [pair:protected] => BTCBTC
        )

    [1] => Uphold\Model\Rate Object
        (
            [ask:protected] => 234.89
            [bid:protected] => 234.8
            [currency:protected] => USD
            [pair:protected] => BTCUSD
        )
)
```

### Create and commit a new transaction
```php
require_once 'vendor/autoload.php';

use Uphold\UpholdClient as Client;

// Initialize the client.
$client = new Client();

// Get user.
$user = $client->getUser('AUTHORIZATION_TOKEN');

// Get a specific card by id.
$card = $user->getCardById('ade869d8-7913-4f67-bb4d-72719f0a2be0');

// Create a new transaction.
$transaction = $card->createTransaction('foo@bar.com', '2.0', 'BTC');

// Commit the transaction.
$transaction->commit();
```

The above produces the output shown below:

```php
Uphold\Model\Transaction Object
(
    [id:protected] => a97bb994-6e24-4a89-b653-e0a6d0bcf634
    [createdAt:protected] => 2015-01-30T11:46:11.439Z
    [denomination:protected] => Array
        (
            [pair] => BTCBTC
            [rate] => 1
            [amount] => 2.0
            [currency] => BTC
        )
    [destination:protected] => <snip>
    [message:protected] =>
    [origin:protected] => <snip>
    [params:protected] => <snip>
    [refundedById:protected] =>
    [status:protected] => completed
    [type] => transfer
)
```

### Get all public transactions
```php
require_once 'vendor/autoload.php';

use \Uphold\UpholdClient as Client;

// Initialize the client.
$client = new Client();

// Get all public transactions (public endpoint).
$pager = $client->getReserve()->getTransactions();

// Get next page of transactions.
$transactions = $pager->getNext();
```

Or you could get a specific public transaction:

```php
// Get one public transaction.
$transaction = $client->getReserve()->getTransactionById('a97bb994-6e24-4a89-b653-e0a6d0bcf634');
```

### Get reserve status
```php
require_once 'vendor/autoload.php';

use \Uphold\UpholdClient as Client;

// Initialize the client.
$client = new Client();

// Get the reserve summary of all the obligations and assets within it (public endpoint).
$statistics = $client->getReserve()->getStatistics();
```

### Pagination

Some endpoints will return a paginator. Here is some examples on how you can handle it.

```php
// Get public transactions.
$pager = $client->getReserve()->getTransactions();

// Check whether the paginator has a valid next page.
while($pager->hasNext()) {
    // Get next page with results.
    $transactions = $pager->getNext();

    // Do something...
}
```

## Tests

Run the tests from the root directory:

    $ ./vendor/bin/phpunit

You can find PHPUnit documentation [here](https://phpunit.de/documentation.html).

## Contributing & Development

#### Contributing

Found a bug or want to suggest something? Take a look first on the current and closed [issues](https://github.com/seegno/uphold-sdk-php/issues). If it is something new, please [submit an issue](https://github.com/seegno/uphold-sdk-php/issues/new).

#### Develop

It will be awesome if you can help us evolve `uphold-sdk-php`. Want to help?

1. [Fork it](https://github.com/seegno/uphold-sdk-php).
2. `php composer.phar install`.
3. Hack away.
4. Run the tests: `phpunit`.
5. Create a [Pull Request](https://github.com/seegno/uphold-sdk-php/compare).
