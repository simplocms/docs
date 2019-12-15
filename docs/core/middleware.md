---
id: middleware
title: Middleware
sidebar_label: Middleware
---

## Introduction

Middleware offers a mechanism for developers, which filtering coming HTTP requests. Laravel includes already some middleware, 
what for example verifies authentication of users or check if incoming CSRF token is valid.

> For more information about **Middleware**, please visit official [Laravel documentation](https://laravel.com/docs/5.8/middleware)

## Middleware in SIMPLO CMS

SIMPLO CMS contains custom middlewares, which are applied out of the box.

In `app/Http/Kernel.php` file, where developers can define middleware, you can see these SIMPLO CMS middlewares too. Including them, you can 
also notice that here are more middleware groups than default of Laravel, what occur here because of SIMPLO CMS.

### Middleware Groups

From lines above, you already know that SIMPLO CMS also contains few middleware groups more than you can find in default of Laravel. 
You can see the whole definition of them in the `app/Http/Kernel.php` file:

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

In SIMPLO CMS, we have four middleware groups, what developers can use - `web`, `front_web`, `resources` and `api`. A part of 
these middleware groups are some default middlewares of Laravel, but also some middlewares are specified for SIMPLO CMS and we will decribe 
them in the next paragraphs.

### Middleware

First SIMPLO CMS middleware is `UserLocale`. This middleware is responsible for setting locale for user, but it works only 
when user is authenticated, because user cannot set his default locale when he is not authenticated. This middleware is 
only in middleware groups (specifically in `web` and `api`) and developer cannot use it as a named middleware.

In middleware groups, you can also find `RedirectIfNotAdmin` middleware, what is special middleware for only SIMPLO CMS. How this 
class name indicates, `RedirectIfNotAdmin` middleware check if user is administrator and if his account is already enabled. This middleware is 
part of `api` middleware group only, but you can find it also between route middlewares. It means that developers can define this middleware 
on some routes in application and ensure only permit access for administrators.

Next middleware, what is also defined in middleware group, is `ApiEndPoint`. It is middleware, what helps to developers with 
making correct api routes and set for these api routes `application/json` header. This middleware is also part of 
`api` middleware group.

The most middlewares are in the `front_web` middleware group. It is completely SIMPLO CMS middleware group, which is not part of 
default Laravel. First middleware is `FrontWeb`, what is base in `front_web` group and this middleware serves a lot of activities:
- bind `FrontWebComposer`, which inserts global properties to each view: `$_FW_SERVICE`, `$_FW_DATA` and `$_FW_HOME_URL`
- check custom redirects
- URL language verification

In `front_web` middleware group except of `FrontWeb`, you can find `InjectFrontWebTools` middleware. It is very simple middleware, 
which adds only CMS Toolbar and do nothing special more.