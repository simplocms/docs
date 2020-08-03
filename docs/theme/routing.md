---
id: routing
title: Routing
---

## Introduction

In Laravel, routes are defined in route files and located in `routes` directory. Out of the box, Laravel has two base 
route files - `web.php` and `api.php`. In `web.php`, there are defined routes for a web interface and implements the `web` 
middleware group. Route file `api.php` offers you to define routes, which are assigned to the `api` middleware group.

> For more information about **Routing**, visit an offical [Laravel documentation](https://laravel.com/docs/5.8/routing)

## Routing

In a theme, routing is the same like in Laravel and you do not need to learn something new. All routes in your theme are defined 
in a base `routes.php` file, which is located in `routes` directory of theme's root.

By default, this `routes.php` file contains the code below:
```php
<?php declare(strict_types = 1);

//use Illuminate\Routing\Route;
//
//Route::group([
//    'middleware' => 'web',
//    'namespace' => 'Theme\Http\Controllers'
//], static function() {
//
//    Route::get('example', [
//        'as' => 'theme.example',
//        'uses' => 'ExampleController@index',
//    ]);
//
//});
```
A good attitude is, when you will not remove this default source code, because this is a convenient route group for definition of 
your routes. The route group already implements the middleware `web` with the namespace `Theme\Http\Controllers`. All theme's controllers 
are located under this namespace.

> **Remember**, when you want to use names of your routes from a theme, then you need to prefix their names with `theme.`. For example, 
> if you want to get full url address to your route from a theme, then you must call `route('theme.your_route_name')`.