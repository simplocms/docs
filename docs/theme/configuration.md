---
id: configuration
title: Configuration
---

## Introduction

After creating a new theme, probably you will want to check and set your configuration files. 
Everything about configuration is located in `config` directory and for the first time, there are two configuring files - `admin.php` and 
`universal_modules.php`.

### Admin Configuration 

In the `admin.php` file, you can set a few following configurations below:

**`name`**

Using this configuration key, you can set the main name of your project.

**`menu_locations`**

The `menu_locations` key accepts an array associative value. It's a convenient place to set all menu locations (positions if it's better understandable for you) in your theme.
As a value, you must set a text, which will represents each menu location. You are free to set only a translation key for this representing text. Everything, what you set here, will be showed
on */admin/menu* page in a setting menu.

**`settings`**

Under the `settings` key, you will see by default the `%lang%_articles_page_id` key. This is a key, which set a type of the primary key for an article page. Probably you will 
never change this default value.

**`ck_editor`**

How you can notice from the key's name, this is a configuration key for a few settings of CKEditor - `heading_options` and `lists`. Under the `heading_options`
key, you can define items, which appear in heading options of CKEditor - for example paragraph or heading. For the `lists` key, it's possible to define
more different lists for CKEditor.

> If you know more about CKEditor configuration rules, check the following **Admin Configuration Example**.

Admin Configuration Example:
```php 
<?php

return [
    'name' => 'Your project name',

    'menu_locations' => [
        'primary' => 'theme::theme.menu_locations.primary',
        'footer' => 'theme::theme.menu_locations.footer'
    ],

    'settings' => [
        '%lang%_articles_page_id' => \App\Services\Settings\SettingsService::TYPE_INT
    ],

    'ck_editor' => [
        'heading_options' => [
            [
                'model' => 'paragraph',
                'title' => 'Paragraph',
                'view' => [
                    'name' => 'p'
                ],
            ],
            [
                'model' => 'heading3',
                'view' => [
                    'name' => 'h3',
                    'classes' => ['vs-h', 'vs-h--tertiary']
                ],
                'title' => 'Heading 3',
            ],
            [
                'model' => 'heading4',
                'view' => [
                    'name' => 'h4',
                    'classes' => ['vs-h', 'vs-h--quaternary']
                ],
                'title' => 'Heading 4',
            ],
            [
                'model' => 'heading5',
                'view' => [
                    'name' => 'h5',
                    'classes' => ['vs-h', 'vs-h--quinary']
                ],
                'title' => 'Heading 5',
            ],
        ],
        'lists' => [
            [
                'name' => 'vs-list',
                'title' => 'Rectangle',
            ]
        ]
    ],

];
```

## Custom Configuration

### Define Custom Configuration

For extending a theme with custom configurations, you can just create a new configuration file in `config` directory and use 
dot syntax the same like in Laravel.

For example, if you want to add a new authorisation guard, then create a new configuration file `config/auth.php` and insert the following code: 

```php
<?php

return [
    
    'auth.guards.new_user' => [
        'driver' => 'session',
        'provider' => 'new_users',
    ],

    'auth.providers.new_users' => [
        'driver' => 'eloquent',
        'model' => \Theme\Models\NewUser::class,
    ],

    'auth.passwords.new_users' => [
        'provider' => 'new_users',
        'table' => 'new_user_password_resets',
        'expire' => 60,
    ],

];
```

> Of course, you are **completely free to define** your configuration source code. You don't need to keep the source code in this example above including the file's name.

**WARNING!** If you want to use custom configuration file, it is necessary to register them using `Theme\Providers\ThemeServiceProvider`, which 
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

For **register a new configuration file**, you need to call `registerSystemConfigFrom` method on `App\Services\ThemeService\ThemeService` object. 
This object is obtained with calling `getThemeService` method in `ThemeServiceProvider`. The `registerSystemConfigFrom()` method accepts only 
one parameter `$key` as a string data type and this is a key, what you defined in your new configuration file (in the previous example above, the key is `auth`).

### How To Use Custom Configuration

After what you created and registered a custom configuration, then you can easily use all of them in your theme everywhere. For use them, just call somewhere `config` 
Laravel helper function with a specific key. 

> **Remember**, that you **do not need to prefix** your custom configuration key with 
> something special (ex. theme's namespace), it is completely the same like in default Laravel.
