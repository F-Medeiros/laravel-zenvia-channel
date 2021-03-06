# Zenvia Notifications Channel for Laravel 5

[![Latest Version on Packagist](https://img.shields.io/packagist/v/luisdalmolin/laravel-zenvia-channel.svg?style=flat-square)](https://packagist.org/packages/luisdalmolin/laravel-zenvia-channel)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Total Downloads](https://poser.pugx.org/luisdalmolin/laravel-zenvia-channel/downloads.png)](https://packagist.org/packages/luisdalmolin/laravel-zenvia-channel)

This package makes it easy to send Zenvia SMS messages using [Zenvia API](http://docs.zenviasms.apiary.io) with Laravel 5.

## Contents

- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
    - [Available Message methods](#available-message-methods)
- [Changelog](#changelog)
- [Testing](#testing)
- [Security](#security)
- [Contributing](#contributing)
- [Credits](#credits)
- [License](#license)

## Installation

You can install the package via composer:

``` bash
composer require luisdalmolin/laravel-zenvia-channel
```

You must install the service provider:

```php
// config/app.php
'providers' => [
    ...
    NotificationChannels\Zenvia\ZenviaServiceProvider::class,
],
```

## Configuration

Configure your credentials: 

```php
// config/services.php
...
'zenvia' => [
    'from'    => env('ZENVIA_FROM', 'Laravel Notification Channels'),
    'pretend' => env('ZENVIA_PRETEND', false),
    'conta'   => env('ZENVIA_CONTA', 'YOUR ACCOUNT'),
    'senha'   => env('ZENVIA_SENHA', 'YOUR PASSWORD')
],
...
```

## Usage

You can now use the channel in your `via()` method inside the Notification class.

``` php
use NotificationChannels\Zenvia\ZenviaChannel;
use NotificationChannels\Zenvia\ZenviaMessage;
use Illuminate\Notifications\Notification;

class InvoicePaid extends Notification
{
    public function via($notifiable)
    {
        return [ZenviaChannel::class];
    }

    public function toZenvia($notifiable)
    {
        return ZenviaMessage::create()
            ->from('Laravel') // optional
            ->to($notifiable->phone) // your user phone
            ->content('Your invoice has been paid')
            ->id('your-sms-id');
    }
}
```

### Routing a message

You can either send the notification by providing with the chat id of the recipient to the to($phone) method like shown in the above example or add a routeNotificationForZenvia() method in your notifiable model:

```php
...
/**
 * Route notifications for the Telegram channel.
 *
 * @return int
 */
public function routeNotificationForZenvia()
{
    return $this->phone;
}
...
```

### Available Message methods

- `to($phone)`: (integer) Recipient's phone.
- `content('message')`: (string) SMS message.
- `from('Sender')`: (string) Sender's name.
- `id('sms-id')`: (string) SMS ID.

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Security

If you discover any security related issues, please email luis.nh@gmail.com instead of using the issue tracker.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Credits

- [Luís Dalmolin](https://github.com/luisdalmolin)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
