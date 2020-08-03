---
id: controllers
title: Controllers
---

## Introduction

Controllers offer you to organize all of your requests in classes. It's a better way how to define a new route handler instead of a Closure
in `routes.php` file. 

> For more details, go to read an official [Laravel documentation](https://laravel.com/docs/5.8/controllers)

## How To Work With Controllers In Module

The work with controllers in a module is the same like in Laravel. For creating a new controller, use `Http/Controllers` path for storing them.
In general the modules serves either for extending your application about a new entity or just extending Grid Editor. Up to purpose of the controller,
this module's controller must extend from `App\Http\Controllers\Admin\AdminController` (for extending an administration panel).

> **IMPORTANT!** Please, **do not use** a module controller for handling a front web. For this purpose, you can use [Theme/Controllers](../theme/controllers.md)

After creating a new module controller, then you need to refresh Composer's autoload from SIMPLO CMS root:
```text 
$ composer dump-autoload
```