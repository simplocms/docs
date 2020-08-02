---
id: general
title: General
---

## Introduction

Modules are a convenient way, how developers can extend their projects built on SIMPLO CMS about new entities. Each
module contains own controllers, views and models and module's work-flow corresponds with Laravel.

For developing with modules, SIMPLO CMS uses popular Laravel's package **[nwidart/laravel-modules](https://github.com/nWidart/laravel-modules)**.
If you know more about this package, it's a good idea to start in this official documentation.

## How Starts to Work With Modules

Let's try to create a first custom example module together - FAQ (Frequently Ask Questions). For create the module, make a copy of some existing module from `modules` folder and rename this new folder to
`FrequentlyAskQuestion`.

> **IMPORTANT! Be careful**, that your own module **must be located in your project's theme folder** inside the `themes` folder. One more, the module's file (controllers, models, service providers, etc.)
> must have namespaces without `Theme` on the first position.

## Namespace

When you create the module, then the module provider (class `ServiceProvider` in `Providers` folder) registers new namespaces also corresponding with the module's name.
For `FrequentlyAskQuestion` example, it's created the namespace with the key named `module-frequently-ask-question`. This namespace is required, when developers want to call the module's resources.

> It's necessary to have in your module's namespace prefix `module-`. SIMPLO CMS will be able to recognized your module correctly.

After these steps, you need to change a namespace of `ServiceProvider`, because you made your new module by copying from the existed one. For our example, you need to use
`Modules\FrequentlyAskQuestion\Providers` namespace.