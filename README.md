# Firebase for Laravel

A Laravel package for the [Firebase PHP Admin SDK](https://github.com/kreait/firebase-php).

[![Current version](https://img.shields.io/packagist/v/kreait/laravel-firebase.svg?logo=composer)](https://packagist.org/packages/kreait/laravel-firebase)
[![Monthly Downloads](https://img.shields.io/packagist/dm/kreait/laravel-firebase.svg)](https://packagist.org/packages/kreait/laravel-firebase/stats)
[![Total Downloads](https://img.shields.io/packagist/dt/kreait/laravel-firebase.svg)](https://packagist.org/packages/kreait/laravel-firebase/stats)
[![Tests](https://github.com/kreait/laravel-firebase/workflows/Tests/badge.svg?branch=main)](https://github.com/kreait/laravel-firebase/actions)
[![codecov](https://codecov.io/gh/kreait/laravel-firebase/branch/main/graph/badge.svg)](https://codecov.io/gh/kreait/laravel-firebase)
[![Discord](https://img.shields.io/discord/523866370778333184.svg?color=7289da&logo=discord)](https://discord.gg/nbgVfty)
[![Sponsor](https://img.shields.io/static/v1?logo=GitHub&label=Sponsor&message=%E2%9D%A4&color=ff69b4)](https://github.com/sponsors/jeromegamez)

- [Installation](#installation)
  - [Laravel](#laravel)
  - [Lumen](#lumen)
- [Configuration](#configuration)
- [Upgrading to version 3](#upgrade)
  - [Facades](UPGRADE.md#facades)
- [Usage](#usage)
  - [Multiple projects](#multiple-projects)
- [Support](#support)
- [License](#license)

## Installation

This package requires Laravel 6.x and higher or Lumen 6.x and higher.

```bash
composer require kreait/laravel-firebase
```

If you use Lumen or don't use Laravel's package auto-discovery, add the following service provider in
`config/app.php` (Laravel) or `bootstrap/app.php` (Lumen):

### Laravel

```php
<?php
// config/app.php
return [
    // ...
    'providers' => [
        // ...
        Kreait\Laravel\Firebase\ServiceProvider::class
    ]
    // ...
];
```

### Lumen

```php
<?php
// bootstrap/app.php

$app->register(Kreait\Laravel\Firebase\ServiceProvider::class);

// If you want to use the Facades provided by the package
$app->withFacades();
```

## Upgrade
See [UPGRADE.md](UPGRADE.md) for upgrade instructions.

## Configuration

In order to access a Firebase project and its related services using a server SDK, requests must be authenticated.
For server-to-server communication this is done with a Service Account.

The package uses auto discovery for the default project to find the credentials needed for authenticating requests to
the Firebase APIs by inspecting certain environment variables and looking into Google's well known path(s).

If you don't already have generated a Service Account, you can do so by following the instructions from the
official documentation pages at https://firebase.google.com/docs/admin/setup#initialize_the_sdk.

Once you have downloaded the Service Account JSON file, you can use it to configure the package by specifying
the environment variable `FIREBASE_CREDENTIALS` in your `.env` file:

```
FIREBASE_CREDENTIALS=/full/path/to/firebase_credentials.json
# or
FIREBASE_CREDENTIALS=relative/path/to/firebase_credentials.json
```

For further configuration, please see [config/firebase.php](config/firebase.php). You can modify the configuration
by copying it to your local `config` directory or by defining the environment variables used in the config file:

```bash
# Laravel
php artisan vendor:publish --provider="Kreait\Laravel\Firebase\ServiceProvider" --tag=config

# Lumen
mkdir -p config
cp vendor/kreait/laravel-firebase/config/firebase.php config/firebase.php
```

## Usage

| Component | [Automatic Injection](https://laravel.com/docs/5.8/container#automatic-injection) | [Facades](https://laravel.com/docs/facades) | [`app()`](https://laravel.com/docs/helpers#method-app) |
| --- | --- | --- | --- |
| [Authentication](https://firebase-php.readthedocs.io/en/stable/authentication.html) | `\Kreait\Firebase\Auth` | `Firebase::auth()` | `app('firebase.auth')` |
| [Cloud Firestore](https://firebase-php.readthedocs.io/en/stable/cloud-firestore.html) | `\Kreait\Firebase\Firestore` | `Firebase::firestore()` | `app('firebase.firestore')` |
| [Cloud&nbsp;Messaging&nbsp;(FCM)](https://firebase-php.readthedocs.io/en/stable/cloud-messaging.html) | `\Kreait\Firebase\Messaging` | `Firebase::messaging()` | `app('firebase.messaging')` |
| [Dynamic&nbsp;Links](https://firebase-php.readthedocs.io/en/stable/dynamic-links.html) | `\Kreait\Firebase\DynamicLinks` | `Firebase::dynamicLinks()` | `app('firebase.dynamic_links')` |
| [Realtime Database](https://firebase-php.readthedocs.io/en/stable/realtime-database.html) | `\Kreait\Firebase\Database` | `Firebase::database()` | `app('firebase.database')` |
| [Remote Config](https://firebase-php.readthedocs.io/en/stable/remote-config.html) | `\Kreait\Firebase\RemoteConfig` | `Firebase::remoteConfig()` | `app('firebase.remote_config')` |
| [Cloud Storage](https://firebase-php.readthedocs.io/en/stable/cloud-storage.html) | `\Kreait\Firebase\Storage` | `Firebase::storage()` | `app('firebase.storage')` |

Once you have retrieved a component, please refer to the [documentation of the Firebase PHP Admin SDK](https://firebase-php.readthedocs.io)
for further information on how to use it.

**You don't need and should not use the `new Factory()` pattern described in the SDK documentation, this is already
done for you with the Laravel Service Provider. Use Dependency Injection, the Facades or the `app()` helper instead**

### Multiple projects

Multiple projects can be configured in [config/firebase.php](config/firebase.php) by adding another section to the projects array.

When accessing components, the facade uses the default project. One can also explicitly use a project:

```php
Firebase::auth() // returns an intance of \Kreait\Firebase\Auth with the configuration found for the default project
// or
// 'app' is configured as the default project by default, this would be equivalent to Firebase::firestore() when that is the case
Firebase::project('app')->firestore() // returns an intance of \Kreait\Firebase\Firestore with the configuration found for the 'app' project
// or
Firebase::project('secondary-app')->database() // returns an instance of \Kreait\Firebase\Database with the configuration found for the 'secondary-app' project
```

## Support

- [Issue Tracker (Laravel Package)](https://github.com/kreait/laravel-firebase/issues/)
- [Issue Tracker (Admin SDK)](https://github.com/kreait/firebase-php/issues/)
- [Discord Chat](https://discord.gg/nbgVfty)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/firebase+php)

## License

Firebase Admin PHP SDK is licensed under the [MIT License](LICENSE).

Your use of Firebase is governed by the [Terms of Service for Firebase Services](https://firebase.google.com/terms/).
