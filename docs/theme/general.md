---
id: general
title: General
---

## How does it work

All themes are placed into `themes` directory in project's root directory. Theme is identifier by name of it's directory.

SIMPLO CMS always needs to have some active theme and for that there is theme named Example, which is fallback theme. When 
there is no active theme, SIMPLO CMS will automatically lookup for theme to activate.

Once the theme is activated, there is created symlink `themes/_active`, which points to directory of activated theme. 
Also, there is created symlink to public directory (`public/theme`), which points to `resources/dist` directory of 
activated theme (if this directory exists of course).

If theme contains composer's autoload (path `vendor/autoload.php` in theme's directory), SIMPLO CMS will automatically 
include this autoload before bootstraping.

## Bootstrapping

Bootstrapping is the process SIMPLO CMS takes theme's service provider to configure all necessities needed for correct functionality.

Service provider is heart of your theme. Basically, it needs two main methods: `register` and `boot`. 

First, the `register` method is called - it's the place, where you can register services (*there is no guarantee here that all 
other providers were loaded yet, so do not try to call other services here!*). 

Once all providers have been registered, the `boot` method will be called. Within the `boot` method you may do whatever 
you like: register view composers, model controllers, override configuration or anything else you can imagine.

> Service provider of your theme should extend `App\Services\ThemeService\Patterns\AbstractThemeServiceProvider`, which will 
automatically boot translations, config and views for you (correctly namespaced). Also it will give you some helper 
functions to ease registering model controllers (`registerModelController`), view composers (`registerViewComposer`), 
custom exception handler (`registerExceptionHandler`) and some more. 

## Directory Structure

Every theme should have at least similar directory structure like this:

```text
.
├── config
|   ├── admin.php
|   └── universal_modules.php
├── database
|   └── migrations
├── resources
|   ├── _static
|   ├── assets
|   |   ├─ js
|   |   └─ scss
|   ├── dist
|   |   ├─ fonts
|   |   ├─ images
|   |   ├─ js
|   |   ├─ css
|   |   └─ mix-manifest.json
|   ├── lang
|   └── views
├── routes
|   └── routes.php
├── src
|   └── Components
|   └── Http
|   |   ├── Controllers
|   |   └── ViewComposers
|   └── Providers
|       └─ ThemeServiceProvider.php
├── vendor
|   └── autoload.php
├── .gitignore
├── package.json
├── package-lock.json
├── webpack.mix.js
├── composer.json
├── composer.lock
└── README.md
```

When theme does not need database, routes, autoload etc., it shouldn't have these directories included (keep only necessary).

Please always include `README.md`, which describes theme purpose, how to make it work, what requirements (other than SIMPLO CMS requirements) 
it has, what third party packages it uses and for what reason.

Also include `package-lock.json` and `composer.lock` files into theme's repository.

## Namespacing

> Responsible namespacing prevents conflicts with SIMPLO CMS classes and resources, and improves readability and clarity of the code.

Theme should have everything namespaced. PHP classes should be under namespace `Theme\\`. Views, configuration and 
translations should be namespaced under `theme::`. Routes are automatically prefixed with `theme.`, and url's for administration 
should be prefixed with `theme/`.
