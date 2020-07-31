---
id: controllers
title: Controllers
---

## Introduction

Controllers offer you to handle all of your requests in one class as methods instead of Closure in route files. It is a correct way 
for clean and robust code. By default, Laravel stores controllers in `app/Http/Controllers` path. 

> For more details, go to read an official [Laravel documentation](https://laravel.com/docs/5.8/controllers)

## How To Define Controllers In Theme

In a theme, you can also choose controllers for handling all requests and store them to `src/Http/Controllers` path of 
your theme. For the controller in a theme is proper, when extends `App\Http\Controllers\FrontWeb\FrontWebBaseController`, what is the base 
controller in SIMPLO CMS stored in `app/Http/Controllers/FrontWeb` path in root directory. This controller provides another convenient
methods, which helpful you during a theme's development such as a `themeView` method, which make view content with a prefix `theme::` for getting 
some theme view simply.

> **IMPORTANT!** Please, **do not use** a theme controller for adding a new entity (or you can understand it also as a model). For this purpose, you can use
> [Modules](../modules/general.md), where you can create models, controllers, database migrations, database seeders, etc. Using modules you can also extend
> the administration panel, where you will manage your future entities.

When you create a new web route using a theme controller, it's a good way when you will specify a page title, a description or another base things. For 
these things, you can use predefined methods, which we will describe together in the following rows below:

### Title

For set a title in HTML `title` element, you can call the following source code inside your controller's method:

```php
<?php

...

    /**
     * GET: List of items
     *
     * @return \Illuminate\Contracts\View\View
     */
    public function list()
    {
        ...

        $this->getFrontWebService()->getFrontWebData()->setTitle('Your Title!');

        ...
    }

...

```

The calling can look quite uncomfortable because of the long chain. If you want to make it shorter, it will be better, when you will define your new theme
basic controller. Then all controllers inside your theme will extend from this new theme basic controller. Check the source code below:

```php
<?php declare(strict_types = 1);

namespace Theme\Http\Controllers;

use App\Contracts\ViewableModelInterface;
use App\Http\Controllers\FrontWeb\FrontWebBaseController;
use App\Services\FrontWeb\FrontWebService;
use App\Structures\StructuredData\Serializer;
use Illuminate\Contracts\View\Factory;

class ThemeFrontWebBaseController extends FrontWebBaseController
{
    /**
     * Constructor
     *
     * @param FrontWebService $frontWebService
     * @param Factory $viewFactory
     */
    public function __construct(FrontWebService $frontWebService, Factory $viewFactory)
    {
        $this->initialize($frontWebService, $viewFactory);
    }

    /**
     * Set title
     *
     * @param string $title
     * @return  void
     */
    public function setTitle(string $title): void
    {
        $this->getFrontWebService()->getFrontWebData()->setTitle($title);
    }
}
```

Then, you can call only `$this->setTitle('Your Title!');` in your controller's method.

> How you can notice, the new theme controller **must extend** from `App\Http\Controllers\FrontWeb\FrontWebBaseController`.

### Breadcrumbs

If you have breadcrumbs on your website, you can use SIMPLO CMS **breadcrumbs system**. For set breadcrumbs, you can call the `setBreadcrumbs` method, which
accepts one required parameter - `App\Structures\Collections\BreadcrumbsCollection`. It's a collection with all available breadcrumbs for a current page. The example below
will show you how to define some breadcrumbs: 

```php 
<?php

...

use App\Structures\Collections\BreadcrumbsCollection;
use App\Structures\DataTypes\Breadcrumb;

...

    /**
     * GET: List of items
     *
     * @return \Illuminate\Contracts\View\View
     */
    public function list()
    {
        ...

        $breadcrumbs = new BreadcrumbsCollection();
        $breadcrumbs->push(
            new Breadcrumb(
                $this->getFrontWebService()->themeTrans('your_trans_key'),
                route('theme.your_route')
            )
        );

        // Set breadcrumbs
        $this->setBreadcrumbs($breadcrumbs);

        ...
    }

...

```

> When you will check a source code of the settings above (title, breadcrumbs), you can notice that the system uses for everything 
> the `App\Services\FrontWeb` class. It's an important service, which provides a lot of options for a few settings of page's metatags, etc.
> If you want to get more information, please visit [Front Web Service](../services/front-web.md). 

After what you created successfully your new controller, then it is necessary to refresh base Composer's autoloader with the command below:
```text 
$ composer dump-autoload
```

It should be noted that there are already several controllers in theme controllers directory - `ArticlesController`, `PagesController` and 
`SearchController`. These controllers provide you a good way for modify some main logic or view for default pages in SIMPLO CMS theme.