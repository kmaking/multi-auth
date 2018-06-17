# KMAKING MultiAuth for Laravel 5.6.*

- `php artisan multi-auth:install {guard} -f`
- `php artisan multi-auth:install {guard} -f --domain`
- `php artisan multi-auth:install {guard} {service} -f --lucid`

## What it does?
With one simple command you can setup multi auth for your Laravel 5.6 project. The package installs:
- Model
- Migration
- Controllers
- Notification
- Routes
  - routes/web.php
    - {guard}/login
    - {guard}/register
    - {guard}/logout
    - password reset routes
  - routes/{guard}.php
    - {guard}/home
- Middleware
- Views
- Guard
- Provider
- Password Broker
- Settings 

## Usage

### Step 1: Install Through Composer

```shell
composer require kmaking/multi-auth --dev
```

### Step 2: Install Multi-Auth files in your project

```
php artisan multi-auth:install {singular_lowercase_name_of_guard} -f

// Examples
php artisan multi-auth:install admin -f
php artisan multi-auth:install employee -f
php artisan multi-auth:install customer -f
```

Notice:
If you don't provide `-f` flag, it will not work. It is a protection against accidental activation.

Alternative:

If you want to install Multi-Auth files in a subdomain you must pass the option `--domain`.
```
php artisan multi-auth:install admin -f --domain
php artisan multi-auth:install employee -f --domain
php artisan multi-auth:install customer -f --domain
```

To be able to use this feature properly, you should add a key to your .env file:
```
APP_DOMAIN=yourdomain.com
```
This will allow us to use it in the routes file, prefixing it with the domain feature from Laravel routing system.

Using it like so: `['domain' => '{guard}.' . env('APP_DOMAIN')]`.

### Step 3: Migrate new model table 

```
php artisan migrate
```

### Step 4: Try it

Go to: `http://url_to_your_project/guard/login`
Example: `http://project/admin/login`

## Options

If you don't want model and migration use `--model` flag.
```
php artisan multi-auth:install admin -f --model
```

If you don't want views use `--views` flag.
```
php artisan multi-auth:install admin -f --views
```

If you don't want routes in your `routes/web.php` file, use `--routes` flag.

```
php artisan multi-auth:install admin -f --routes
```

## Note
If you want to adapt the redirect path once your `guard` is logged out, add and override the following method in
your {guard}Auth\LoginController:

```php
/**
 * Get the path that we should redirect once logged out.
 * Adaptable to user needs.
 *
 * @return string
 */
public function logoutToPath() {
    return '/';
}
```

## Files which are changed and added by this package
- config/auth.php
  - add guards, providers, passwords

- app/Http/Providers/RouteServiceProvider.php
  - register routes

- app/Http/Kernel.php
  - register middleware

- app/Http/Middleware/
  - middleware for each guard

- app/Http/Controllers/{Guard}Auth/
  - new controllers

- app/{Guard}.php
  - new Model
  
- app/Notifications/{Guard}ResetPassword.php
  - reset password notification

- database/migrations/
  - migration for new model

- routes/web.php
  - register routes

- routes/{guard}.php
  - routes file for given guard

- resources/views/{guard}/
  - views for given guard
  
## Changelog

### Note: Never install configurations with same guard again after installed new version of package. So if you already installed your `admin` guard, don't install it again after you update package to latest version. 

## Note

#### This package was originaly from `Hesto/multi-auth`, we improved only routes, controller and view files.
