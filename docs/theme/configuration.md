---
id: configuration
title: Configuration
---

After creating new theme (the best way for creating new theme is just copy Example theme), probably you will want to check your configuration files. 
Everything about configuration is located in `config` directory and in first time there are two configuration files - `admin.php` and 
`universal_modules.php`.

> TODO: add information about these files above !!!

## Custom Configuration

### Define Custom Configuration

For extending theme with custom configuration files, you can just create new configuration files in `config` directory and use 
dot syntax the same like in Laravel for define configuration keys.

For example, if you want to add new authorization guard, then create new configuration file `config/auth.php` and insert the following code: 

```php
<?php

return [
    
    'auth.guards.your_new_user' => [
        'driver' => 'session',
        'provider' => 'your_new_users',
    ],

    'auth.providers.your_new_users' => [
        'driver' => 'eloquent',
        'model' => \Theme\Models\YourNewUser::class,
    ],

    'auth.passwords.your_new_users' => [
        'provider' => 'your_new_users',
        'table' => 'your_new_user_password_resets',
        'expire' => 60,
    ],

];
```

> Of course, you are **completely free to define** your configuration source code and you don't need to keep the source code in this example.

**WARNING!** If you want to use custom configuration files, it is necessary to register them using `Theme\Providers\ThemeServiceProvider`, which 
is located in `src/Providers` path. By default `ThemeServiceProvider`, you can see the following source code below:

```php
<?php declare(strict_types = 1);

namespace Theme\Providers;

use App\Services\ThemeService\Patterns\AbstractThemeServiceProvider;
use Theme\Http\ViewComposers\MainLayoutComposer;
use Theme\Components\SettingsFormFactory;

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
    }
}
```

For **register new configuration file**, you need to call `registerSystemConfigFrom()` method on `App\Services\ThemeService\ThemeService` object. 
This object is obtained with calling `getThemeService()` method in `ThemeServiceProvider`. The `registerSystemConfigFrom()` method accepts only 
one parameter `$key` as a string data type and this is key, what you defined in your new configuration file (in our example, the key is `auth`).

### How To Use Custom Configuration

After what you create and register custom configuration, then you can easily use all of them in your theme everywhere. For use them, just call somewhere `config()` 
helper function with specific key, for example `config('your.key')`. **Remember**, that you **don't need to prefix** your custom configuration key with 
something special, it is completely the same like in default Laravel.
