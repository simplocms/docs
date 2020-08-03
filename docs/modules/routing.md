---
id: routing
title: Routing
---

## Introduction 

In Laravel, routes are defined in route files and located in `routes` directory. Out of the box, Laravel has two base 
route files - `web.php` and `api.php`. In `web.php`, there are defined routes for a web interface and implements the `web` 
middleware group. Route file `api.php` offers you to define routes, which are assigned to the `api` middleware group.

Laravel offers for developers a simple way how to define a route. All routes are stored in the `routes` directory. By default,
Laravel consists of two base route files - `web.php` and `api.php`. Inside the `web.php` route file, there are routes which will use
for a web interface under the `web` middleware group. In the `api.php` route file, it's possible to make a new route, which will
serve for API.

> For more information about **Routing**, visit an offical [Laravel documentation](https://laravel.com/docs/5.8/routing)

## How To Define Routes For Module

In a module, you can find the `Http/routes.php` file for purpose of definition all module's routes. Everything is the same like in Laravel.
You can set a few options for module's routes as a route name, a closure, a controller, etc. 

Your `routes.php` file in your custom module can look like the following example:
```php
<?php

Route::group([
    'middleware' => 'web',
    'prefix' => 'admin/module/frequently-ask-question',
    'namespace' => 'Modules\FrequentlyAskQuestion\Http\Controllers\Admin',
    'as' => 'module.frequently_ask_question.'
], function () {

    Route::group([
        'prefix' => 'question',
        'as' => 'question.'
    ], function () {

        Route::get('/', [
            'as' => 'index',
            'uses' => 'FrequentlyAskQuestionsController@index',
        ]);

    });

});
```
A good attitude is when you will define in a module just only admin routes. In general, modules are serves mainly for extending your application
about a new entity (or Grid Edtior). Using a module, you can extend an administration panel.

> **Remember**, when you want to use names of your routes from a module, then you need to prefix their names with `module.module_name`. For example, 
> if you want to get full url address to your module's route, then you must call `route('module.module_name.your_route_name')`.