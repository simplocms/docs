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

It is also possible to copy some existing module and make changes how you need. Below, you can see advantages and disadvantages:

#### Using Artisan Command

**Advantages**

- All files are generated for the specific module, so they have the correct namespaces

**Disadvantages**

- After gererating, module's Service Provider is not set properly for using with **SIMPLO CMS** and you have to set everything
manually

#### Just Make A Copy

**Advantages**

- Module's Service Provider is set for using with **SIMPLO CMS**, it's only need to change module's namespace key

**Disadvantages**

- It's necessary to rename all namespaces in all files

It's up to you which way is convenient for you. We recommend you to just make a copy of some existing default 
module in **SIMPLO CMS**, because with **using Artisan Command** you need to set module's Service Provider and it's more complicated.