---
id: middleware
title: Middleware
sidebar_label: Middleware
---

## Introduction

Middleware offers a mechanism for developers, which filtering coming HTTP requests. Laravel includes already some middleware, 
what for example verifies authentication of users or check if CSRF token is valid.

> For more information about Middleware, please visit official [Laravel documentation](https://laravel.com/docs/5.8/middleware)

## Middleware in SimploCMS

SimploCMS contains custom middlewares, which are applied out of the box.

In `app/Http/Kernel.php` file, where developers can define middleware, you can see these middlewares. Including them, you can 
also notice that here are more middleware groups than default of Laravel, what occur here because of SimploCMS.

```php
    /**
     * The application's route middleware groups.
     *
     * @var array
     */
    protected $middlewareGroups = [
        'web' => [
            \Illuminate\Session\Middleware\StartSession::class,
            \App\Http\Middleware\EncryptCookies::class,
            \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
            \Illuminate\View\Middleware\ShareErrorsFromSession::class,
            \App\Http\Middleware\VerifyCsrfToken::class,
            \Illuminate\Routing\Middleware\SubstituteBindings::class,

            \App\Http\Middleware\UserLocale::class,
        ],

        'front_web' => [
            \App\Http\Middleware\FrontWeb::class,
            \App\Http\Middleware\InjectFrontWebTools::class,
            // keep this as last middleware, so it handles headers from other singletons
            \App\Http\Middleware\ManageResponse::class,
            \App\Http\Middleware\PrefetchModels::class,
        ],

        'resources' => [
            // nothing yet
        ],

        'api' => [
            \Illuminate\Session\Middleware\StartSession::class,
            \App\Http\Middleware\EncryptCookies::class,
            \Illuminate\Routing\Middleware\ThrottleRequests::class . ':60,1',
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
            \App\Http\Middleware\RedirectIfNotAdmin::class,

            \App\Http\Middleware\UserLocale::class,
            \App\Http\Middleware\ApiEndpoint::class,
        ],
    ];
```