# Meta Pixel & Conversions API for Laravel

[![Latest Version on Packagist](https://img.shields.io/packagist/v/combindma/laravel-facebook-pixel.svg?style=flat-square)](https://packagist.org/packages/combindma/laravel-facebook-pixel)
[![License](https://img.shields.io/packagist/l/combindma/laravel-facebook-pixel.svg?style=flat-square)](LICENSE.md)

A Laravel package for integrating **Meta Pixel (Facebook Pixel)** and the **Meta Conversions API (CAPI)** into your application.

It enables synchronized browser-side and server-side event tracking with automatic event deduplication, advanced matching, and a clean Laravel API.

---

## Features

- ✅ Meta Pixel browser event tracking
- ✅ Meta Conversions API (CAPI) integration
- ✅ Automatic event deduplication using shared `event_id`
- ✅ Advanced Matching (SHA-256 hashing)
- ✅ Laravel Facade support
- ✅ Blade components for Pixel installation
- ✅ Middleware support for redirect-based tracking
- ✅ Session flash handling for Purchase and Lead events
- ✅ Laravel configuration publishing
- ✅ Test Event Code support
- ✅ Production-ready architecture

---

## Requirements

- PHP 8.2+
- Laravel 10.x / 11.x / 12.x

---

# Installation

Install the package using Composer.

```bash
composer require combindma/laravel-facebook-pixel
```

Publish the configuration file.

```bash
php artisan vendor:publish --tag=meta-pixel-config
```

---

# Configuration

Add the following variables to your `.env` file.

```env
META_PIXEL_ID=
META_PIXEL_TOKEN=
META_PIXEL_APP_SECRET=

META_PIXEL_ENABLED=true
META_PIXEL_ADVANCED_MATCHING_ENABLED=true

# Optional
META_TEST_EVENT_CODE=
```

---

# Blade Installation

Include the components inside your layout.

```blade
<!DOCTYPE html>
<html>
<head>
    <x-metapixel-head />
</head>

<body>

    <x-metapixel-body />

    {{ $slot }}

</body>
</html>
```

---

# Usage

## Browser + Server Tracking (Recommended)

This method sends the browser Pixel event and the server-side Conversions API event using the same `event_id`, allowing Meta to automatically deduplicate the event.

```php
use Combindma\FacebookPixel\Facades\MetaPixel;
use FacebookAds\Object\ServerSide\CustomData;

$customData = (new CustomData())
    ->setCurrency('USD')
    ->setValue(125.50);

MetaPixel::trackAndSend(
    eventName: 'Purchase',
    browserData: [
        'currency' => 'USD',
        'value' => 125.50,
    ],
    customData: $customData,
    eventId: 'ORDER-' . time()
);
```

---

## Server-Side Only (Conversions API)

```php
use Combindma\FacebookPixel\Facades\MetaPixel;
use FacebookAds\Object\ServerSide\CustomData;

$userData = MetaPixel::userData()
    ->setEmail('customer@example.com')
    ->setPhone('8801712345678');

$customData = (new CustomData())
    ->setCurrency('BDT')
    ->setValue(2500);

MetaPixel::send(
    'Lead',
    'lead-' . uniqid(),
    $customData,
    $userData
);
```

---

## User Data

Generate Meta-compatible hashed user information.

```php
$userData = MetaPixel::userData()
    ->setEmail('customer@example.com')
    ->setPhone('8801712345678')
    ->setFirstName('John')
    ->setLastName('Doe');
```

---

# Event Deduplication

When using both Meta Pixel and the Conversions API, both requests must share the same `event_id`.

This package automatically supports Meta's recommended deduplication workflow.

```php
MetaPixel::trackAndSend(
    eventName: 'Purchase',
    browserData: [...],
    customData: $customData,
    eventId: 'ORDER-1001'
);
```

---

# Advanced Matching

Advanced Matching hashes customer identifiers using SHA-256 before transmitting them to Meta.

Supported identifiers include:

- Email
- Phone
- First Name
- Last Name
- City
- State
- Country
- ZIP Code

---

# Testing

Run the package test suite.

```bash
composer test
```

You can also configure a Meta Test Event Code.

```env
META_TEST_EVENT_CODE=TEST12345
```

---

# Package Structure

```
src/
├── Facades/
│   └── MetaPixel.php
├── Http/
├── Middleware/
├── View/
├── MetaPixel.php
└── helpers.php

config/
└── meta-pixel.php
```

---

# Supported Events

The package supports any standard Meta event.

Examples include:

- PageView
- ViewContent
- Search
- AddToCart
- InitiateCheckout
- AddPaymentInfo
- Purchase
- Lead
- CompleteRegistration
- Contact
- Subscribe
- Donate
- CustomizeProduct

---

# Roadmap

- [ ] Laravel Pulse integration
- [ ] Event Queue support
- [ ] Batch Event API
- [ ] Multi Pixel support
- [ ] Event Logging
- [ ] Analytics Dashboard
- [ ] Laravel Octane optimization

---

# Contributing

Contributions are welcome.

Please open an issue before submitting large changes.

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Submit a Pull Request

---

# Security

If you discover a security vulnerability, please open a private security report instead of creating a public issue.

---

# License

This package is licensed under the MIT License.

See the [LICENSE](LICENSE.md) file for details.

---

# Author

**Md. Awal Bashar**

Full Stack Laravel & E-commerce Engineer

- GitHub: https://github.com/bashar0091
- LinkedIn: https://www.linkedin.com/in/awalbashar/
- Portfolio: https://bashar0091.github.io/awalbasharofficial/