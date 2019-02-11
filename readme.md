## Laravel 4.2 Framework (Kernel) Fork
> **Note:** This repository contains a fork of the Laravel framework core code. Laravel 4.2 is no longer officially supported. If you want to build an application using Laravel, visit the main [Laravel repository](https://github.com/laravel/laravel).

## Laravel PHP Framework
Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable, creative experience to be truly fulfilling. Laravel attempts to take the pain out of development by easing common tasks used in the majority of web projects, such as authentication, routing, sessions, queueing, and caching.

Laravel is accessible, yet powerful, providing powerful tools needed for large, robust applications. A superb inversion of control container, expressive migration system, and tightly integrated unit testing support give you the tools you need to build any application with which you are tasked.

## Laravel 4.2 with OpenSSL
Starting in PHP 7, the mcrypt extension is deprecated. The extension has been *abandonware* for nearly a decade now, and was also fairly complex to use. It has therefore been deprecated in favour of OpenSSL, where it will be removed completely from PHP core in 7.2.

This repository forks mainline Laravel 4.2, and replaces features reliant on mcrypt with OpenSSL. **Laravel >= 5.0 uses OpenSSL**.

## Upgrading your existing application
This fork is mostly a drop-in replacement for Laravel 4.2. However it does borrow some features from *Laravel >= 5.0*, such as requiring [DotEnv](https://github.com/vlucas/phpdotenv).

#### 1. Create a `.env` file in the root
Copy the below to a new file, which should be located in the root of your Laravel 4.2 project. 
```
# Application
APP_ENV=local
APP_KEY=
```
  

#### 2. Load DotEnv in `/bootstrap/start.php`
The top of the file should look like the snippet below, replace the previous.
```
<?php

/*
|--------------------------------------------------------------------------
| Create The Application
|--------------------------------------------------------------------------
|
| The first thing we will do is create a new Laravel application instance
| which serves as the "glue" for all the components of Laravel, and is
| the IoC container for the system binding all of the various parts.
|
*/

$app = new Illuminate\Foundation\Application;

/*
|--------------------------------------------------------------------------
| Load DotEnv
|--------------------------------------------------------------------------
|
| Loads variables from .env
|
*/
try {
	(new \Dotenv\Dotenv(realpath(__DIR__.'/../')))->load();
} catch (\Dotenv\InvalidPathException $e) {
	//
}

/*
|--------------------------------------------------------------------------
| Detect The Application Environment
|--------------------------------------------------------------------------
|
| Laravel takes a dead simple approach to your application environments
| so you can just specify a machine name for the host that matches a
| given environment, then we will automatically detect it for you.
|
*/

$env = $app->detectEnvironment(function() {
	return (env('APP_ENV')) ? env('APP_ENV') : 'production';
});
```
  

#### 3. Update configuration in `/config/app.php`
Update the Encryption section to the below:
```
/*
|--------------------------------------------------------------------------
| Encryption Key
|--------------------------------------------------------------------------
|
| This key is used by the Illuminate encrypter service and should be set
| to a random, 32 character string, otherwise these encrypted strings
| will not be safe. Please do this before deploying an application!
|
*/

'key' => env('APP_KEY'),

'cipher' => 'AES-256-CBC',
```

#### 4. Link the fork into your project
Add this fork to the repositories section your `composer.json` and run `composer upgrade`

```
"repositories": {
    "laravel/framework": {
        "type": "vcs",
        "url": "https://github.com/engageinteractive/laravel-4-improved-encryption"
    }
}
```
  
#### 5. Generate a new application key
You should run the Artisan generate key command, which will input a compliant application key into your `.env` file

`php artisan key:generate`

## Official Documentation
Documentation for the framework can be found on the [Laravel website](http://laravel.com/docs).

### License
The Laravel framework is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
