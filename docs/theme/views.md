---
id: views
title: Views
---

## Introduction

Views provides you a good way for creating presentation documents (mainly HTML), what are separated from logic side the most 
often defined in controllers. By default, Laravel stores all views in `resources/views` directory.

> For more details, you can visit official [Laravel documentation](https://laravel.com/docs/5.8/views)

## How To Create Views

Views in theme is the same like in Laravel. Actually, the view store is also the same and you will find it in `resources/views` in 
theme's root. For development, it is possible to use all [Laravel Blade](https://laravel.com/docs/5.8/blade) functions and components 
and you aren't limited at all. When you will visit this path with views in first time, there are already a few view folders and files. 
All there is a layout for basic pages and parts of these pages and you are free to change them how you want and how you need.

After when some view is created, you can return it in theme's controller using `themeView` method:
```php
<?php declare(strict_types = 1);

namespace Theme\Http\Controllers;

use App\Http\Controllers\FrontWeb\FrontWebBaseController;
use Illuminate\Contracts\View\View;

final class FooController extends FrontWebBaseController
{
    /**
     * Default contructor
     */
    public function __construct()
    {
        //
    }

    /**
     * Show foo view.
     *
     * @return \Illuminate\Contracts\View\View
     */
    public function showFooIndex(): View
    {
        return $this->themeView('foo.index');
    }
}
```
When you call `themeView` method, then it will return view in your theme's directory `resources/views`. In this method, it will be added prefix 
`theme::` to your view automatically so you don't need to do something more. But if you want to use Laravel `view` helper function because of 
some reason, then you need to write prefix explicitly - `view('theme::foo.index')`. 

> If you want to use comfortable `themeView` method, then it is **necessary** to your controller extends `App\Http\Controllers\FrontWeb\FrontWebBaseController`, 
which provides this method.

## Describe Default Views

In the last section of this documentation page, we met with creating views for theme. How you also know, in `resources/views` path 
are some default view files in first time.

The default structure of views directory looks like this:
```text
resources/views
├── admin
|   └── _settings_form.blade.php
├── articles
|   └── detail.blade.php
├── homepage
|   └── index.blade.php
├── layouts
|   └── main.blade.php
├── menus
|   └── primary.blade.php
├── modules
|   └── articles_list
|       └── list.blade.php
├── pages
|   ├── articles.blade.php
|   ├── page.blade.php
|   └── search.blade.php
└── vendor
    └── breadcrumbs.blade.php
```

> **!!!! TODO: Write descriptions of these view files above !!!!**

> **!!!! TODO: Write more information about global properties in views, how to set meta tags and another things !!!!**