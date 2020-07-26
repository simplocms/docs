---
id: general
title: General
---

## Introduction

Modules are a convenient way, how developers can extend their projects built on SIMPLO CMS about new features. Each
module contains own controllers, views and models and module's work-flow corresponds with Laravel.

For developing with modules, SIMPLO CMS uses popular Laravel's package **[nwidart/laravel-modules](https://github.com/nWidart/laravel-modules)**.
If you know more about this package, it's a good idea to start in this official documentation.

## How Starts to Work With Modules

Let's try to create a first custom example module together. For create a module, make a copy of some existing module from `modules` folder and make 
changes how you need. 

> **IMPORTANT! Be careful**, that your own module **must be located in your project's theme folder** inside the `themes` folder. One more, the module's file (controllers, models, service providers, etc.)
> must have `Modules\Blog\Providers` namespace without `Theme` on the first position.

## Namespace

When you create a new module, then the module provider registers new namespaces also corresponding with the module's name.
For `Blog` example, it's created the namespace with the key named `module-blog`. This namespace is required, when developers want to call
some the module's resources.

For **lang** resources just call `trans('module-blog::name.key');`.

For **view** resources just call `view('module-blog::view');`.

For **config** resources just call `config('module-blog.name.key');`.

> It's necessary to have in your module's namespace prefix `module-`. SIMPLO CMS will be able to recognized your module correctly.