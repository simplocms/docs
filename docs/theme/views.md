---
id: views
title: Views
---

## Introduction

Views provide you a good way for creating presentation documents (mainly HTML), what are separated from logic side the most 
often defined in controllers. By default, Laravel stores all views in `resources/views` directory.

> For more details, you can visit an official [Laravel documentation](https://laravel.com/docs/5.8/views)

## How To Create Views

Views in a theme is the same like in Laravel. Actually, the view store is also the same. You will find it in `resources/views` in 
theme's root. For development, it is possible to use all [Laravel Blade](https://laravel.com/docs/5.8/blade) functions and components 
and you aren't limited at all. When you will visit this path with views in first time, there are already a few view folders and files. 
All there is a layout for default pages and parts of these pages. You are free to change them how you want and how you need.

After when some view is created, you can return it in a theme's controller using `themeView` method:
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
When you call the `themeView` method, then it will return view in your theme's directory `resources/views`. In this method, it will be added prefix 
`theme::` to your view automatically, you do not need to do something more. If you want to use Laravel `view` helper function because of 
some reason, then you need to write a prefix explicitly - `view('theme::foo.index')`. 

> If you want to use comfortable `themeView` method, then it is **necessary** to extending your controller from `App\Http\Controllers\FrontWeb\FrontWebBaseController`, 
which provides this method. Basically, it's a **recommend way** for your all controllers to extend from `App\Http\Controllers\FrontWeb\FrontWebBaseController`.

## Describe Default Views

In the last section of this documentation page, we met with creating views for a theme. How you also know, in `resources/views` path 
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

### Admin 

#### `_settings_form.blade.php`

This is view, what is returned in `createView` method of `Theme\Components\SettingsFormFactory` class. This component implements
`App\Services\ThemeService\Contracts\ThemeSettingsFormFactoryInterface` interface, which is binded by application in `App\Services\ThemeService\Patterns\AbstractThemeServiceProvider`
after calling method `registerSettingsFormFactory`. In a theme, the `registerSettingsFormFactory` method is called in `Theme\Providers\ThemeServiceProvider`, which just extends
`App\Services\ThemeService\Patterns\AbstractThemeServiceProvider` and it means that this provider can call this specific method for registering `Theme\Components\SettingsFormFactory` for
your theme.

> If you want to know how works Service Provider and how you can make your binding, check an official [Laravel Documentation](https://laravel.com/docs/5.8/container). 

The `Theme\Components\SettingsFormFactory` provides `createView` method and using this method, application renders the `_settings_form.blade.php` view into Administration "[Settings > General](../getting-started/setting#general)". It means that developer 
can implement more setting options specially for a theme, which will extend default SIMPLO CMS setting options.

### Articles 

#### `detail.blade.php`
This view serves for showing of the article detail. It's rendered using by the `show` method of `Theme\Http\Controllers\ArticlesController`.

### Homepage 

#### `index.blade.php`
This view is a default view for showing a homepage in a theme. It's rendered in the `show` method of `Theme\Http\Controllers\PagesController`, when
the current page is a homepage.

### Layouts 

#### `main.blade.php`

This is a main layout view, which is extended in all views of theme (of course, you do not need to extend only from this main layout). 
By default, the source code contains basic html document structure and main view properties: 
- `$_FW_DATA` - class `\App\Services\FrontWeb\FrontWebService`
- `$_FW_SERVICE` - class `\App\Services\FrontWeb\FrontWebData`

These properties are attached into this layout main view using composer `App\Http\ViewComposers\FrontWebComposer`, which is registered in 
`App\Http\Middleware\FrontWeb`, and offers a lot of useful methods for your work with:
- language translations
- assets
- create menus
- breadcrumbs
- title
- meta tags - robots, description, twitter, open graph
- canonical url
- structured data (ex. rich snippets)

> For more information about View Composers, visit an official [Laravel Documentation](https://laravel.com/docs/5.8/views#view-composers).
> If you need to get more information about **Front Web Service**, please visit [this page](../services/front-web.md).

### Menus 

#### `primary.blade.php`
This view is rendering in `Theme\Components\View\PrimaryMenu` component and if you can notice in main layout view, the view component is called here.
The `primary.blade.php` view serves for importing primary menu, which is possible to easily set through [Administration Panel](../getting-started/setting.md).

> **The view component** serves more robust solution how developers can implement the same additional source code for a view in one place.

### Modules 

#### `articles_list/list.blade.php`
TODO TODO TODO

### Pages 

#### `articles.blade.php`
This view is returned in method `getArticlesPageView` of `Theme\Http\Controllers\PagesController`. This controller's method shows page of articles and
if you want, you can modify `getArticlesPageView` logic how you need for your theme and its specific purposes.

#### `page.blade.php`
This view is default view file for returning in base `show` method in `Theme\Http\Controllers\PagesController`. TODO ...

#### `search.blade.php`
This view is returned in method `index` of `Theme\Http\Controllers\SearchController` and this method contains logic for searching on theme's website. Everything here, you can
modify how you want, it's completely free to make custom searching logic specific for theme's purpose.

### Vendor 

#### `breadcrumbs.blade.php`
This is view file, which renders breadcrumb menu items in all another views, where it will be imported using `theme::vendor.breadcrumbs` key. By default, 
breadcrumb view is imported into the `pages.articles`, `pages.page`, `pages.search` and `articles.detail` and it's completely free to modify how developers need.
