---
id: middleware
title: Middleware
---

## Introduction

Middleware is a mechanism in Laravel framework, which can be able to filter coming HTTP requests. For more information, please
visit a [Middleware](../core/middleware.md) documentation page.

## How To Create A Middleware In Theme

You are able to create a middleware in your theme, if you need it. By default, you cannot find any middleware directory, but you are
free to create this directory everywhere inside your theme directory. Anyway we recommend you to create this directory for storing a middleware
as `src/Http/Middleware`. It's a default path for Laravel framework, so it's convenient to keep this strategy.

In theme, making a middleware is the same like in Laravel. Just create a middleware file in `src/Http/Middleware` with a required `handle` method,
insert your source code here, and save everything.

After that, you can already apply this middleware for your routes. For example inside a controller, you can use the following code below:

```php 
<?php declare(strict_types = 1);

namespace Theme\Http\Controllers;

use Theme\Http\Middleware\YourMiddleware;

...

    /**
     * Create a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();

        $this->middleware(YourMiddleware::class);
    }

...

```

For set an alias of your middleware, go to `Theme\Providers\ThemeServiceProvder` and insert the source code below: 

```php 
<?php declare(strict_types = 1);

namespace Theme\Providers;

use App\Services\ThemeService\Patterns\AbstractThemeServiceProvider;
use Theme\Http\ViewComposers\MainLayoutComposer;
use Theme\Components\SettingsFormFactory;

use Theme\Http\Middleware\YourMiddleware;

final class ThemeServiceProvider extends AbstractThemeServiceProvider
{
    /**
     * Boot theme.
     */
    public function boot(): void
    {
        parent::boot();

        // Register system configuration
//        $this->getThemeService()->registerSystemConfigFrom('cms');

        // View composers
        $this->registerViewComposer('layouts.main', MainLayoutComposer::class);
        $this->registerSettingsFormFactory(SettingsFormFactory::class);

        $this->registerModelControllers([
            \App\Models\Page\Page::class => \Theme\Http\Controllers\PagesController::class
        ]);

        $this->registerMiddleware();
    }

    /**
     * Register middleware.
     *
     * @return void
     */
    private function registerMiddleware()
    {
        /** @var Router $router */
        $router = app('router');

        $router->aliasMiddleware('your_middleware', YourMiddleware::class);
    }
}

```

When you set an alias of your middleware, you can use this alias for applying your middleware instead of a class name. For sure, a controller is not only
one place, where you can apply a middleware. If it's more convenient, you can use a route specification for your middleware or some another way.