---
id: complete-guide
title: Complete Guide
---

## Introduction

With modules, developers can extend an application and SIMPLO CMS about new entities or just extend only Grid Editor.
If you are interesting about these things and how you can make them, you are on the right page. Let's develop them together.

## Complete Guide - Make A New Entity

For this complete guide, we will make together one complex module. What about **Frequently Ask Questions**? Are you ready?
Let's do this!

### First Steps

For the first step, we need to make a module's directory with everything what we will need for the module's developing. The best and
the easiest way is just make a copy of some existing module. Unfortunately by default, SIMPLO CMS consists of only modules, which are
designed for Grid Editor. So because of this fact, we can make a copy from any module what we want. For our example, let's make the copy from
`ArticlesList` module located in SIMPLO CMS `modules` directory. We will make this copy to our theme's directory `modules`.

> **IMPORTANT!** Never make your own module inside **SIMPLO CMS** `modules` directory. If you will do it, then you **cannot easily to
> versioning this module** with your theme's project or **do the future updates of SIMPLO CMS without any troubles**.

After making the copy, we need to make some basic changes for starting our developing. First open `Assets/js` directory and remove 
all Javascript files there. 

After that, open the `config.php` module's file in `Config` directory. You have to see the following source code:

```php
<?php

return [
    'name' => 'ArticlesList',
    'icon' => 'list-alt',

    'grideditor_title' => 'module-articleslist::admin.grideditor_title'
];
```

For our module `FrequentlyAskQuestion`, we can have for example the following starting configuration:

```php
<?php

return [
    'name' => 'FrequentlyAskQuestion',
    'icon' => 'comments',

    'for-grideditor' => false,  // we make the module for extending because of a new entity, no designed for Grid Editor

    'admin' => [
        'menu_group_icon' => 'fa fa-comments',
        'menu' => [],           // now, we do not have any items in an administration menu of our module
        'permissions' => [
            'group' => 1        // permission group is '1' because we make a new entity and it's necessary to have permissions in show, create, edit, delete, all areas
        ],
    ]
];
```

> For more information about a module's configuration, you can visit [Modules/Configuration](modules/configuration.md)

When we will set everything, then we can to move `Database/Migrations` path. This directory must be empty so we will remove all migrations there. The same case is also the next
folder of the module's root - `Dist`. This folder is for storing all public resources as Javascript or CSS files.

After the previous steps, we need to keep empty `Http/Controllers`, `Http/Requests` and `Models` directories as well. About `Http/routes.php` file, we do not notice it now.

Now, it's very important step we must do it now. Do it well, because then you can have troubles during the module's developing. Open `Providers/ServiceProvider.php` and check the
current source code:

```php
<?php

namespace Modules\ArticlesList\Providers;

use Illuminate\Contracts\Support\DeferrableProvider;
use Illuminate\Support\ServiceProvider as BaseProvider;

class ServiceProvider extends BaseProvider implements DeferrableProvider
{
    /**
     * Namespace of module resources and configuration.
     * @var string
     */
    protected $namespace = 'module-articleslist';

    /**
     * Boot the application events.
     *
     * @return void
     */
    public function boot()
    {
        $this->registerTranslations();
        $this->registerConfig();
        $this->registerViews();
    }

    /**
     * Register the service provider.
     *
     * @return void
     */
    public function register()
    {
        //
    }

    /**
     * Register config.
     *
     * @return void
     */
    protected function registerConfig()
    {
        $this->mergeConfigFrom(
            __DIR__.'/../Config/config.php',
            $this->namespace
        );
    }

    /**
     * Register views.
     *
     * @return void
     */
    public function registerViews()
    {
        $this->loadViewsFrom(
            __DIR__.'/../Resources/views',
            $this->namespace
        );
    }

    /**
     * Register translations.
     *
     * @return void
     */
    public function registerTranslations()
    {
        $this->loadTranslationsFrom(__DIR__ .'/../Resources/lang', $this->namespace);
    }

    /**
     * Get the services provided by the provider.
     *
     * @return array
     */
    public function provides()
    {
        return [];
    }
}
```

In this source code of `Providers/ServiceProvider.php`, change a namespace to `Modules\FrequentlyAskQuestion\Providers`. Then, you need to change `module-articleslist` to
`module-frequentlyaskquestion`. Everything else is fine. You do not need to change anything more if it's not necessary for your module.

> You can have the namespace of your module how you want of course, but it's necessary to write it everywhere with `module-` prefix. SIMPLO CMS can recognize your module well and 
> the system will work with the module properly.

For `Resources` directory, you will keep only the following directory structure:

```text
.
├── Resources
|   └── lang
|       └── en
|           └── admin.php
|   └── views
|       └── admin
```

When you need to have more languages, you are free to make it inside the `lang` directory inside a new language directory. For
this step, we will modify `admin.php` file and return an empty array from this translation file.

After everything above, now we need to update already only a few files in our new module directory. The first one is `composer.json`, where we can have something 
similar like the source code below:

```json
{
    "name": "admin-module/faq",
    "description": "Module for managing frequently asked questions",
    "version": "1.0.0",
    "authors": [
        {
            "name": "Simplo.cz",
            "email": "info@simplo.cz"
        }
    ],
    "autoload": {
        "psr-4": {
            "Modules\\FrequentlyAskQuestion\\": ""
        }
    }
}
```

In this `composer.json` file, it's very important to write `psr-4` autoload to our new module correctly. Everything here will be just used for
Composer's autoload.

The next file for updating is `module.json`. There are a lot of important options for our new module. Everything there, we can set with the following values:

```json
{
    "name": "FrequentlyAskQuestion",
    "alias": "frequentlyaskquestion",
    "description": "Module for managing FAQ",
    "keywords": [],
    "active": 1,
    "order": 0,
    "providers": [
        "Modules\\FrequentlyAskQuestion\\Providers\\ServiceProvider"
    ],
    "aliases": {},
    "files": [
        "start.php"
    ],
    "requires": []
}
```

> Very useful `module.json` key is `providers`. It's a right place where you can define all providers of your module. For example there you can define a service provider for
> [Modules/Events](modules/events.md).

The next file in a root directory of our new module is `package.json`. When you will open this file, you will see few third-part libraries as dependencies for NPM. You can change it how you
want, but probably if you like to develop with Vue.js, it's not necessary to change anything here for first step in starting of the developing.

Then, it's a good way when you will completely change `README.md` file and make some description of the new module. Because we made a copy from `ArticlesList`, we have no current description there.
Please pay attention for this description of the new module and modules, what you will make also in the future. It's necessary to help for another developers how this module works and what this module
extends (and mainly for which purpose this module is designed).

The last file, what you need to change, is `webpack.mix.js`. Of course only in a case that you like to use Laravel Mix. If yes, then change this file and remove the `.js('Assets/js/configuration.js', 'Dist/configuration.js')` 
source code. For our starting position, we have not defined any assets yet.

Now just for a summary, here is the actual directory structure for our starting position of the module's developing:

```text
.
├── Assets
|   └── js
├── Config
|   └── config.php
├── Database
|   └── Migrations
├── Dist
├── Http
|   ├── Controllers
|   ├── Requests
|   └── routes.php
├── Models
├── Providers
|   └── ServiceProvider.php
├── Resources
|   ├── lang
|   |   └── en
|   |       └── admin.php
|   └── views
├── .gitignore
├── composer.json
├── module.json
├── package.json
├── README.md
├── start.php
└── webpack.mix.js
```

If you have **the same directory structure** for our example module **Frequently Ask Question**, CONGRATULATIONS! Then you can continue
in the next steps about our module's developing with us!

