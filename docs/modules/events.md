---
id: events
title: Events
---

## Introduction

You can make events and their listeners for your own module. The work with events and listeners in modules is the same
like in Laravel. If you want to get more information about this feature, please visit an official [Laravel Documentation](https://laravel.com/docs/5.8/events).

## Registering Events In Module

For make a new event, run the following commands below:

```text
$ php artisan module:make-event BlogPostWasUpdated Blog
$ php artisan module:make-listener NotifyAdminOfNewPost Blog
```

After when you will make your own event, you must register it in Laravel. How you can do it? Very simple! You have two ways, how you can do it:
- manually call and write this source code to your module's `ServiceProvider`:
```php
$this->app['events']->listen(BlogPostWasUpdated::class, NotifyAdminOfNewPost::class);
```

- or a second way is creating a new service provider - `EventServiceProvider`:
```php
<?php

namespace Modules\Blog\Providers;

use Illuminate\Foundation\Support\Providers\EventServiceProvider as ServiceProvider;

class EventServiceProvider extends ServiceProvider
{
    protected $listen = [];
}
```

The second way is convenient when you have more events in your module. Then you can manage all events in one place. Don't forget
that this `EventServiceProvider` must extends from `Illuminate\Foundation\Support\Providers\EventServiceProvider`. After what you specify
and make everything needed in your own module's `EventServiceProvider`, add this class in `module.json` file for loading an instance provider.