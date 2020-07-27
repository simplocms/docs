---
id: general
title: General
---

## What Is It?

Theme is a project's template, where you will develop your own project. All time during your developing, you will spend this time in your
theme's project directory, nowhere else.

> **It's very important** to have your custom code only in your theme's project directory because of **the future updates of SIMPLO CMS!**
> If you use a version system as GIT, you will version only this project's folder.


## How Does It Work?

All themes are located in the `themes` directory. Each theme is identified by its directory's name.

SIMPLO CMS always needs to have some active theme and for this purpose, there is the theme named Example, which is fallback theme. When 
there is no active theme, SIMPLO CMS will automatically lookup for this theme to activate.

Once a theme is activated, there is created symlink `themes/_active`, which points to directory of activated theme. 
Also, there is created symlink to public directory (`public/theme`), which points to `resources/dist` directory of 
activated theme (if this directory exists for sure).

If a theme contains composer's autoload (path `vendor/autoload.php` in theme's directory), SIMPLO CMS will automatically 
include this autoload before bootstrapping.

## Create A New Theme

The best and easiest way for creating a new theme is just make a copy of a default theme - **Example**. After this copying, you will have everything available for
developing.

## Directory Structure

Each theme should have at least similar directory structure like this:

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
├── webpack.mix.js
├── composer.json
└── README.md
```

When a theme does not need database, routes, autoload etc., it shouldn't have these directories included (keep only necessary).

Please always include `README.md`, which describes a theme's purpose, how to make it works, what requirements (other than SIMPLO CMS requirements) 
this theme has, which third party packages the theme uses and for what reason.

## Bootstrapping

Bootstrapping is the process SIMPLO CMS takes theme's service provider to configure all necessities needed for correct functionality.

Service provider is heart of your theme. Basically, it needs two main methods: `register` and `boot`. 

First, the `register` method is called - it's the place, where you can register services. It is not guarantee that all 
other providers were loaded yet, so you must not try to call any other services in this method. 

Once all providers have been registered, the `boot` method will be called. Within the `boot` method you may do whatever 
you like: register view composers, model controllers, override configuration or anything else what you can imagine.

> Service provider of your theme **should extend** from `App\Services\ThemeService\Patterns\AbstractThemeServiceProvider`, which will 
automatically boot translations, configurations and views with correct namespaces. It will also give you some helper 
functions to ease registering model controllers (`registerModelController`), view composers (`registerViewComposer`), 
custom exception handler (`registerExceptionHandler`), etc. 

## Namespacing

> Responsible namespacing prevents conflicts with SIMPLO CMS classes and resources, and improves readability and clarity of the code.

Theme should have everything namespaced. PHP classes should be under the namespace `Theme\\`. Views, configurations and 
translations should be namespaced under `theme::`. Routes are automatically prefixed with `theme.`.
