# Customer.io API Client

PHP bindings for the Customer.io API.

[API Documentation](https://www.customer.io/docs/api/)

[![Actions Status](https://github.com/printu/customerio/workflows/PHP%20Composer/badge.svg?branch=master)](https://github.com/printu/customerio/actions)
[![Code Climate](https://codeclimate.com/github/printu/customerio/badges/gpa.svg)](https://codeclimate.com/github/printu/customerio)
[![Test Coverage](https://codeclimate.com/github/printu/customerio/badges/coverage.svg)](https://codeclimate.com/github/printu/customerio/coverage)

There are two primary API hosts available for to integrate with:

**Behavioral Tracking**

https://track.customer.io/api/v1/ \
Behavioral Tracking API is used to identify and track customer data with Customer.io.

**API**

https://api.customer.io/v1/api/ \
API allows you to read data from your Customer.io account for use in custom workflows in your backend system or for reporting purposes.

## Installation

The API client can be installed via [Composer](https://github.com/composer/composer).

In your composer.json file:

```json
{
    "require": {
        "printu/customerio": "~3.0"
    }
}
```

Once the composer.json file is created you can run `composer install` for the initial package install and `composer update` to update to the latest version of the API client.

The client uses [Guzzle](http://docs.guzzlephp.org/en/stable/).

## Basic Usage

Remember to include the Composer autoloader in your application:

```php
<?php
require_once 'vendor/autoload.php';

// Application code...
?>
```

Configure your access credentials when creating a client:

```php
<?php
use Customerio\Client;

$client = new Client('YOUR_API_KEY', 'YOUR_SITE_ID');

/*
 * To authenticate, provide your key as a Bearer token in a HTTP Authorization header.
 * You can create and manage your API keys by visiting your App API Keys page directly or by clicking the Integrations
 *  link in the left-hand menu of your Customer.io account and choosing Customer.io API > Manage API Credentials > App API Keys.
 */
$client->setAppAPIKey('APP_KEY');

?>
```

Change region to EU

```php
<?php
use Customerio\Client;

$client = new Client('YOUR_API_KEY', 'YOUR_SITE_ID', ['region' => 'eu']);

?>
```

### Local Testing

Run `phpunit` from the project root to start all tests.

### Examples

#### Customers

```php
<?php
// Create customer
try {
    $client->customers->add(
        [
            'id' => 1,
            'email' => 'user@example.com',
            'plan' => 'free',
            'created_at' => time()
        ]
    );
} catch (\GuzzleHttp\Exception\GuzzleException $e) {
    // Handle the error
}

// Get customer
try {
    $client->customers->get(
        [
            'email' => 'user@example.com',        
        ]
    );
} catch (\GuzzleHttp\Exception\GuzzleException $e) {
    // Handle the error
}

// Update customer
try {
    $client->customers->update(
        [
            'id' => 1,
            'email' => 'user@example.com',
            'plan' => 'premium'
        ]
    );
} catch (\GuzzleHttp\Exception\GuzzleException $e) {
    // Handle the error   
}

// Delete customer
try {
    $client->customers->delete(
        [
            'id' => 1,
        ]
    );
} catch (\GuzzleHttp\Exception\GuzzleException $e) {
    // Handle the error   
}
```

#### Events

```php
<?php
// Add customer event
try {
    $client->customers->event(
        [
            'id' => 1,
            'name' => 'test-event',
            'data' => [
                'event-metadata-1' => 'test',
                'event-metadata-2' => 'test-2'
            ]
        ]
    );
} catch (\GuzzleHttp\Exception\GuzzleException $e) {
    // Handle the error
}

// Add anonymous event
try {
    $client->events->anonymous(
        [
            'name' => 'invite-friend',
            'data' => [
                'recipient' => 'invitee@example.com'
            ]
        ]
    );
} catch (\GuzzleHttp\Exception\GuzzleException $e) {
    // Handle the error
}
```

Anonymous event [example](http://customer.io/docs/invitation-emails.html) usage.

#### Segments
```php
<?php
// Get segment data
try {
    $client->segments->get(
        [
            'id' => 1
        ]
    );
} catch (\GuzzleHttp\Exception\GuzzleException $e) {
    // Handle the error
}
```

Check for other available methods [here](https://customer.io/docs/api/#apibeta-apisegmentssegments_list)

#### PageView

```php
<?php
// Add page view
try {
    $result = $client->page->view(
        [
            'id' => 1,
            'url' => 'http://example.com/login',
            'data' => [
                'referrer' => 'http://example.com'
            ]
        ]
    );
} catch (\GuzzleHttp\Exception\GuzzleException $e) {
    // Handle the error
}
```

#### Campaigns

```php
<?php
// Get campaigns data
try {
    $client->campaigns->get(
        [
            'id' => 1
        ]
    );
} catch (\GuzzleHttp\Exception\GuzzleException $e) {
    // Handle the error
}
```

Check for other available methods [here](https://customer.io/docs/api/#apibeta-apicampaignscampaigns_get)

```php
<?php
// Trigger broadcast campaign
try {
    $result = $client->campaigns->trigger(
        [
            'id' => 1,
            'data' => [
                'headline' => 'Roadrunner spotted in Albuquerque!',
                'date' => 'January 24, 2018', 
                'text' => 'We\'ve received reports of a roadrunner in your immediate area! Head to your dashboard to view more information!' 
            ],
            'recipients' => [
                'segments' => [
                    'id' => 1
                ]
            ]
        ]
    );
} catch (\GuzzleHttp\Exception\GuzzleException $e) {
    // Handle the error
}
```

See [here](https://learn.customer.io/documentation/api-triggered-data-format.html) for more examples of API Triggered Broadcasts

## License

MIT license. See the [LICENSE](LICENSE) file for more details.