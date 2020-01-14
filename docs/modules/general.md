---
id: general
title: General
---

## Introduction

Modules are a convenient way, how developers can extend their projects built on SIMPLO CMS about new features. Every
modules contains own controllers, views and models and module's work-flow corresponds with Laravel.

For developing with modules, SIMPLO CMS uses popular Laravel's package **[nwidart/laravel-modules](https://github.com/nWidart/laravel-modules)**.
If you know more about this package, it's good idea to visit official documentation.

## How Starts to Work With Modules

Let's try to create a first custom example module together. For create a module, here is available [Artisan Console](core/artisan-console.md) command:

```text
php artisan module:make Blog
```

After running this Artisan Command, the folder structure of this new module will be following:

```text
modules
└── Blog
    ├── Config
    |   └── config.php
    ├── Database
    |   ├── Migrations
    |   └── Seeders
    |       └── BlogDatabaseSeeder.php
    ├── Http
    |   ├── Controllers
    |   |   └── BlogController.php
    |   ├── Middleware
    |   ├── Requests
    |   └── routes.php
    ├── Media
    ├── Models
    ├── Providers
    |   └── BlogServiceProvider.php
    ├── Resources
    |   ├── lang
    |   └── views
    |       ├── layouts
    |       |   └── master.blade.php
    |       └── index.blade.php
    ├── composer.json
    ├── module.json
    └── start.php
```