---
id: complete-guide
title: Complete Guide
---

## Introduction

With modules, developers can extend an application and SIMPLO CMS about new entities or just extend only Grid Editor.
If you are interesting about these things and how you can make them, you are on the right page. Let's develop them together.

For this complete guide, we will describe a lot of features, what SIMPLO CMS offers for developers with modules.

## First Steps

For the first step, we need to make a module's directory with everything what we will need for the module's developing. The best and
the easiest way is just make a copy of some existing module. Unfortunately by default, SIMPLO CMS consists of only modules, which are
designed for Grid Editor. So because of this fact, we can make a copy from any module what we want. For our example, let's make the copy from
`ArticlesList` module located in SIMPLO CMS `modules` directory. We will make this copy to our theme's directory `modules`.

> **IMPORTANT!** Never make your own module inside **SIMPLO CMS** `modules` directory. If you will do it, then you **cannot easily to
> versioning this module** with your theme's project or **do the future updates of SIMPLO CMS without any troubles**.

After making the copy, we need to make some basic changes for starting our developing. First open `Assets/js` directory and remove 
all Javascript files there. 

After that, open the `config.php` module's file in `Config` directory. You have to see the following source code:

```php
<?php

return [
    'name' => 'ArticlesList',
    'icon' => 'list-alt',

    'grideditor_title' => 'module-articleslist::admin.grideditor_title'
];
```

For our module, we can have for example the following starting configuration:

```php
<?php

return [
    'name' => 'ModuleName',
    'icon' => 'comments',

    'for-grideditor' => false,  // we make the module for extending because of a new entity, no designed for Grid Editor

    'admin' => [
        'menu_group_icon' => 'fa fa-comments',
        'menu' => [],           // now, we do not have any items in an administration menu of our module so far
        'permissions' => [
            'group' => 1        // permission group is '1' because we make a new entity and it's necessary to have permissions in show, create, edit, delete, all areas
        ],
    ]
];
```

> For more information about a module's configuration, you can visit [Modules/Configuration](modules/configuration.md)

When we will set everything, then we can to move `Database/Migrations` path. This directory must be empty, so we will remove all migrations there. The same case is also the next
folder of the module's root - `Dist`. This folder is for storing all public resources as Javascript or CSS files.

After the previous steps, we need to keep empty `Http/Controllers`, `Http/Requests` and `Models` directories as well. About `Http/routes.php` file, we do not notice it now.

Now, it's very important step we must do it now. Do it well, because then you can have troubles during the module's developing. Open `Providers/ServiceProvider.php` and check the
current source code:

```php
<?php

namespace Modules\ArticlesList\Providers;

use Illuminate\Contracts\Support\DeferrableProvider;
use Illuminate\Support\ServiceProvider as BaseProvider;

class ServiceProvider extends BaseProvider implements DeferrableProvider
{
    /**
     * Namespace of module resources and configuration.
     * @var string
     */
    protected $namespace = 'module-articleslist';

    /**
     * Boot the application events.
     *
     * @return void
     */
    public function boot()
    {
        $this->registerTranslations();
        $this->registerConfig();
        $this->registerViews();
    }

    /**
     * Register the service provider.
     *
     * @return void
     */
    public function register()
    {
        //
    }

    /**
     * Register config.
     *
     * @return void
     */
    protected function registerConfig()
    {
        $this->mergeConfigFrom(
            __DIR__.'/../Config/config.php',
            $this->namespace
        );
    }

    /**
     * Register views.
     *
     * @return void
     */
    public function registerViews()
    {
        $this->loadViewsFrom(
            __DIR__.'/../Resources/views',
            $this->namespace
        );
    }

    /**
     * Register translations.
     *
     * @return void
     */
    public function registerTranslations()
    {
        $this->loadTranslationsFrom(__DIR__ .'/../Resources/lang', $this->namespace);
    }

    /**
     * Get the services provided by the provider.
     *
     * @return array
     */
    public function provides()
    {
        return [];
    }
}
```

In this source code of `Providers/ServiceProvider.php`, change a namespace to `Modules\ModuleName\Providers`. Then, you need to change `module-articleslist` to
`module-modulename`. Everything else is fine. You do not need to change anything more if it's not necessary for your module.

> You can have the namespace of your module how you want of course, but it's necessary to write it everywhere with `module-` prefix. SIMPLO CMS can recognize your module well and 
> the system will work with the module properly.

> When you will not use views or translations for your module, then it's a convenient way to remove these register methods.

For `Resources` directory, you will keep only the following directory structure:

```text
.
├── Resources
|   └── lang
|       └── en
|           └── admin.php
|   └── views
|       └── admin
```

When you need to have more languages, you are free to make it inside the `lang` directory inside a new language directory. For
this step, we will modify `admin.php` file and return an empty array from this translation file.

After everything above, now we need to update already only a few files in our new module directory. The first one is `composer.json`, where we can have something 
similar like the source code below:

```json
{
    "name": "admin-module/modulename",
    "description": "Your module description",
    "version": "1.0.0",
    "authors": [
        {
            "name": "Simplo.cz",
            "email": "info@simplo.cz"
        }
    ],
    "autoload": {
        "psr-4": {
            "Modules\\ModuleName\\": ""
        }
    }
}
```

In this `composer.json` file, it's very important to write `psr-4` autoload to our new module correctly. Everything here will be just used for
Composer's autoload. Using `composer.json` you are able to use third-party libraries as well.

The next file for updating is `module.json`. There are a lot of important options for our new module. Everything there, we can set with the following values:

```json
{
    "name": "ModuleName",
    "alias": "modulename",
    "description": "Your module description",
    "keywords": [],
    "active": 1,
    "order": 0,
    "providers": [
        "Modules\\ModuleName\\Providers\\ServiceProvider"
    ],
    "aliases": {},
    "files": [
        "start.php"
    ],
    "requires": []
}
```

- **name** - it is a key which must be set to the same name like a module's directory - format CamelCase
- **alias** - it is a key which must be set to the same name like a module's directory - format lowecase
- **active** - it can have either `1` or `0` value and this key indicates if the module can be used or no
- **providers** - there can be defined all providers for the module
- **files** - it is an array of files which are used for initialization of the module. It's necessary to be here `start.php`
- **requires** - it is a list of another modules which this module requires for its functionality

> Very useful `module.json` key is `providers`. It's a right place where you can define all providers of your module. For example there you can define a service provider for
> [Modules/Events](modules/events.md).

The next file in a root directory of our new module is `package.json`. When you will open this file, you will see few third-part libraries as dependencies for NPM. You can change it how you
want, but probably if you like to develop with Vue.js, it's not necessary to change anything here for first step in starting of the developing.

Then, it's a good way when you will completely change `README.md` file and make some description of the new module. Because we made a copy from `ArticlesList`, we have no current description there.
Please pay attention for this description of the new module and modules, what you will make also in the future. It's necessary to help for another developers how this module works and what this module
extends (and mainly for which purpose this module is designed).

The last file, what you need to change, is `webpack.mix.js`. Of course only in a case that you like to use Laravel Mix. If yes, then change this file and remove the `.js('Assets/js/configuration.js', 'Dist/configuration.js')` 
source code. For our starting position, we have not defined any assets yet.

Now just for a summary, here is the actual directory structure for our starting position of the module's developing:

```text
.
├── Assets
|   └── js
├── Config
|   └── config.php
├── Database
|   └── Migrations
├── Dist
├── Http
|   ├── Controllers
|   ├── Requests
|   └── routes.php
├── Models
├── Providers
|   └── ServiceProvider.php
├── Resources
|   ├── lang
|   |   └── en
|   |       └── admin.php
|   └── views
├── .gitignore
├── composer.json
├── module.json
├── package.json
├── README.md
├── start.php
└── webpack.mix.js
```

#### Assets 

It's a directory where are stored all asset resource files - ex. javascript, scss, etc. after their compilation, compiled files of
these resource files will appear in `Dist` directory. By the way it's possible to store something directly inside `Dist` directory
when the asset file is already compiled and ready for use in html.

#### Dist 

It's compiled source files about assets. You can insert here your source assets directly if these files are compiled and ready for use in html.
If this directory is created in the module, then after an installation process of module it will be created a symlink with
`public/modules/{module_name}` path linked to this module's `Dist` directory. To the assets you can access with `asset('modules/{NazevModulu}/image.jpeg')`
helper function. If you want to use **Laravel Mix**, then it's possible to use `$module->mix('main.js')` method calling, where `$module` is an instance of
`App\Models\Module\Module` class. This instance we can get using for example `$module = \Module::find("MyModule")`.

> If a module is installed already and after that you will create `Dist` directory, it's necessary to reinstall this module
> for creating a symlink to there and get your assets public.

#### Config 

It's a directory for storing all configuration files. Each configuration file must be registered in theme `ServiceProvider`.

#### Database 

It's a directory for storing database migrations and seeders. The same principle like in Laravel.

#### Http 

It's a directory which can contain of controllers, requests, routes, middleware, etc. All controllers for administration
should be stored under the `Admin` directory and namespace, except of `ModuleController`.

> The `ModuleController` is used for Grid Editor module only. For getting some information about this kind of module, you can
> visit [Modules / For Grid Editor](for-grid-editor.md)

#### Models 

It's a directory for models.

#### Providers 

It's a place for storing providers. Don't forget to register them inside the `module.json` file.

#### Resources 

It's a location for storing all language and view files.

If you have **the same directory structure** for our example module **Your Module**, CONGRATULATIONS! Then you can continue
in the next steps about our module's developing with us!

> If you want, **you can use this starting position also for another modules**.

## How To Make - Database, Models, Controllers, Compiling Assets, Events

Before the following rows, you need to get more, how to define a database migration, a model, a controller, an event or how to compiling
assets. For these information, you can visit the following pages:
- Database - [link](modules/database.md)
- Models - [link](modules/models.md)
- Routing - [link](modules/routing.md)
- Controllers - [link](modules/controllers.md)
- Compiling Assets - [link](modules/compiling-assets.md)
- Events - [link](modules/events.md)

The best idea is when you will read all pages above. Then if you will know everything important, you can continue in this **Complete Guide**.

## Basic Implementation

After what you have read everything above, and you will know how to make a database migration, a model, a controller and another things
for your model, you are ready to learn how you can implement your module into the administration of **SIMPLO CMS**.

For a database migration and a model, there is nothing necessary for basic implementation in **SIMPLO CMS**. Just define them how you want.

When you made them, let's go on together. First, it's a good attitude for define few routes. Open `Http/routes.php` file inside your module and up to what you will need, it's
a good way to have something similar like the source code below:

```php
<?php

Route::group([
    'middleware' => 'web',
    'prefix' => 'admin/module/your-module',
    'namespace' => 'Modules\YourModule\Http\Controllers\Admin',
    'as' => 'module.your_module.'
], function () {

    Route::group([
        'prefix' => 'your-entity',
        'as' => 'your_entity.'
    ], function () {

        Route::get('/', [
            'as' => 'index',
            'uses' => 'YourEntityController@index',
        ]);
    
        Route::get('create', [
            'as' => 'create',
            'uses' => 'YourEntityController@create',
        ]);
    
        Route::post('store', [
            'as' => 'store',
            'uses' => 'YourEntityController@store',
        ]);
    
        Route::get('edit/{item}', [
            'as' => 'edit',
            'uses' => 'YourEntityController@edit',
        ]);
    
        Route::patch('update/{item}', [
            'as' => 'update',
            'uses' => 'YourEntityController@update',
        ]);
    
        Route::delete('delete/{item}', [
            'as' => 'delete',
            'uses' => 'YourEntityController@delete',
        ]);

    });

});
```

Of course, you can have there more routes if you need them. But the routes above, it's a good attitude for implementing few basic routes
for an editable entity with show, create, edit, delete areas.

> If you will have **more editable entities in one module**, then **the best way for definition of their routes** is just creating them in **more route groups**. Everything's up to
> your decision. You are free to make your routes how you want, but **do not forget** on the middleware `web`, the prefix url `admin/module/your-module` and the alias `module.your_module` for
> better understandable code.

After definition of the routes, create a controller with these methods from routes. It's a good attitude when you will make `Admin` directory in `Http/Controllers` and inside this directory, you will create
this controller.

```php
<?php

namespace Modules\YourModule\Http\Controllers\Admin;

use App\Http\Controllers\Admin\AdminController;

class YourEntityController extends AdminController
{
    /**
     * YourModuleController constructor.
     */
    public function __construct()
    {
        parent::__construct();

        $this->middleware('permission:module_yourmodule-show')
            ->only(['index']);

        $this->middleware('permission:module_yourmodule-create')
            ->only(['create', 'store']);

        $this->middleware('permission:module_yourmodule-edit')
            ->only(['edit', 'update']);

        $this->middleware('permission:module_yourmodule-delete')
            ->only('delete');
    }

    /**
     * GET: Index
     *
     * @param \Illuminate\Http\Request $request
     * @return \Illuminate\View\View
     */
    public function index(Request $request)
    {
        // TODO: we will create the source code later
    }

    /**
     * GET: Create
     *
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function create()
    {
        // TODO: we will create the source code later
    }

    /**
     * POST: Store
     *
     * @return \Illuminate\Http\RedirectResponse
     */
    public function store()
    {
        // TODO: we will create the source code later
    }

    /**
     * GET: Edit
     *
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function edit()
    {
        // TODO: we will create the source code later
    }

    /**
     * PATCH: Update
     *
     * @return \Illuminate\Http\RedirectResponse
     */
    public function update()
    {
        // TODO: we will create the source code later
    }

    /**
     * DELETE: Delete
     *
     * @return \Illuminate\Http\JsonResponse|\Illuminate\Http\RedirectResponse
     * @throws \Exception
     */
    public function delete()
    {
        // TODO: we will create the source code later
    }
}
```

> **Do not forget** that the controller for extending of the administration in **SIMPLO CMS** must extend from `App\Http\Controllers\Admin\AdminController`.

The source code above, it's a good starting position for our first entity controller in a module. Now, we will write the source code
inside the methods. For the first step, let's implement `index`.

**`YourEntityController::index()`**

This request handle method shows a table which will consists of items from database. Before implementing a source code here, we need to
create a datatable component. Go to `Components/DataTables` folder of your module (if it does not exist, make it) and create a new 
datatable component there - `YourEntityTable`.

```php
<?php

namespace Modules\YourModule\Components\DataTables;

use App\Components\DataTables\AbstractDataTable;
use App\Models\User;
use App\Structures\DataTable\FilterOptions;
use Illuminate\Support\Collection;
use Modules\YourModule\Models\YourEntity;

class YourEntityTable extends AbstractDataTable
{
    /** @var \App\Models\User */
    private $user;

    /**
     * YourEntityTable constructor.
     */
    public function __construct(User $user)
    {
        $this->user = $user;

        parent::__construct();
    }

    /**
     * Initialize datatable.
     */
    protected function initialize(): void
    {
        $this->createColumn('name', trans('module-yourmodule::your-entity/form.labels.name'))
            ->makeSortable(); // it means that this column will be able to use for sorting

        $this->setActionsVisibility($this->user->can(['module_yourmodule-edit', 'module_yourmodule-delete']));
    }

    /**
     * Get data query.
     *
     * @param \App\Structures\DataTable\FilterOptions $filterOptions
     * @return \Illuminate\Database\Query\Builder
     */
    protected function getDataQuery(FilterOptions $filterOptions)
    {
        $query = YourEntity::query();

        // check active a column for sorting
        switch ($filterOptions->getSortingColumn()) {
            case 'name':
                // - `sort` method has one optional parameter `$column`, which we do not have to specify if
                // we want to sort items with the same column name as the name in a database table of the entity
                $filterOptions->sort();
                break;
        }

        // with this method, we can specify columns for searching
        $filterOptions->searchOnColumns('name');
        return $query;
    }

    /**
     * Fill table with fetched data.
     *
     * @param \Illuminate\Support\Collection|YourEntity[] $items
     * @return void
     */
    protected function fill(Collection $items): void
    {
        foreach ($items as $item) {
            $row = $this->addRow($item->getKey());

            $routeParams = [
                'item' => $item->id
            ];

            // Edit
            if ($this->user->can('module_yourmodule-edit')) {
                $row->setDoubleClickAction(route('module.your_module.entity.edit', $routeParams));
                $row->addControl(
                    trans('module-yourmodule::entity/form.btn_edit'),
                    route('module.your_module.entity.edit', $routeParams),
                    'pencil-square-o'
                );
            }

            // Delete
            if ($this->user->can('module_yourmodule-delete')) {
                $row->addControl(
                    trans('module-yourmodule::entity/general.table.btn_delete'),
                    route('module.your_module.entity.delete', [$item->getKey()]),
                    'trash'
                )->setDelete(trans('module-yourmodule::entity/general.confirm_delete'));
            }

            // Preview
            $row->addControl(trans('module-yourmodule::entity/general.table.btn_preview'), $item->full_url, 'eye')
                ->setTarget('_blank');

            // Columns
            $row->addColumn('name', $item->name);
        }
    }
}
```

After create the datatable component, we can finish an implementation of `index` method. It will be simple and we can use
just something similar below:

```php
...

use Modules\YourModule\Components\DataTables\YourEntityTable;

...

    /**
     * GET: Index
     *
     * @param \Illuminate\Http\Request $request
     * @return \Illuminate\View\View
     */
    public function index(Request $request)
    {
        $this->setTitleDescription(
            trans('module-yourmodule::entity/admin.pages.index.title'),
            trans('module-yourmodule::entity/admin.pages.index.description'));

        $table = new YourEntityTable($this->getUser());
        return $table->toResponse($request, 'module-yourmodule::admin.entity.index');
    }

...
```

After this implementation, you need to create the `module-yourmodule::admin.entity.index` view. In the module's directory `admin/entity`
(if this path does not exist then make it) create the `index` view there. In this view, there will be the source code below:

```html
<?php /** @var \Modules\YourModule\Components\DataTables\YourEntityTable $table */ ?>
@extends('admin.layouts.master')

@section('content')
  <v-datatable :table="{{ $table->toJson() }}"></v-datatable>

  @permission('module_yourmodule-create')
    <a href="{{ route('module.your_module.entity.create') }}" class="btn bg-teal-400 btn-labeled">
      <b class="fa fa-pencil-square-o"></b> {{ trans('module-yourmodule::entity/form.btn_create') }}
    </a>
  @endpermission
@endsection

@section('breadcrumb-elements')
  @permission('module_yourmodule-create')
    <li>
      <a href="{{ route('module.module_yourmodule.entity.create') }}">
        <i class="fa fa-pencil-square-o"></i> {{ trans('module-yourmodule::entity/form.btn_create') }}
      </a>
    </li>
  @endpermission
@endsection
```

The datatable will be loaded by **Vue.js** in the `index` view (`v-datatable` vue.js component).

> For getting more about **Vue.js**, you can visit [the official documentation](https://vuejs.org/).

It's everything what you need to do in the `index` view for your module entity. For sure, you can modify this view how you need.

**`YourEntityController::create()`**

The `create` method shows a form for creating a new entity item. Before this implementation, we will need to create a form component. Move to the
`Components/Form` directory and create there `YourEntityForm` class. A source code can look like the source code below:

```php
<?php

namespace Modules\YourModule\Components\Forms;

use App\Components\Forms\AbstractForm;
use Modules\YourModule\Models\YourEntity;

class YourEntityForm extends AbstractForm
{
    /**
     * View name.
     *
     * @var string
     */
    protected $view = 'module-yourmodule::admin.entity.form';

    /**
     * Your Entity.
     *
     * @var YourEntity
     */
    protected $entity;

    /**
     * Mix Manifest Directory
     *
     * @var mixed
     */
    protected $mixManifestDirectory = 'modules/YourModule';

    /**
     * Your Entity form.
     *
     * @param YourEntity $entity
     * @throws \Exception
     */
    public function __construct(YourEntity $entity)
    {
        parent::__construct();
        $this->entity = $entity;

        $this->addScript(url('plugin/js/bootstrap-maxlength.js'));
        $this->addScript(mix('entities.form.js', $this->mixManifestDirectory));
    }

    /**
     * Get view data.
     *
     * @return array
     */
    public function getViewData(): array
    {
        $formAttributes = [
            'name'
        ];

        return [
            'entity' => $this->entity,
            'formValuesJson' => $this->entity->getFormAttributesJson($formAttributes),
            'submitUrl' => $this->entity->exists ?
                route('module.your_module.entity.update', ['item' => $this->entity]) :
                route('module.your_module.entity.store'),
            'cancelUrl' => route('module.your_module.entity.index'),
        ];
    }
}
```

> How you can notice in the source code above, this form component **will be used for create and edit** as well.

For this form component, we need to create the `entities.form.js` javascript file and the `module-yourmodule::admin.entity.form` view.
This javascript file will be stored in `Assets/js` folder with the basic source code below:

```js
import Form from '../../../../../../resources/assets/js/vendor/Form';

Vue.component('yourmodule-entity-form', {
    data() {
        return {
            form: new Form({
                ...this.item
            })
        };
    },

    props: {
        item: {
            type: Object,
            required: true
        }
    }

});
```

You can extend this code how you need. Let's make the form view:

```html
<?php /** @var \Modules\YourModule\Models\YourEntity $item */ ?>
@extends('admin.layouts.master')

@section('content')
    <yourmodule-entity-form inline-template
                           :item="{{ $formValuesJson }}"
                           v-cloak>
        <div class="box-body">
            <v-form :form="form"
                    method="{{ $item->exists ? 'PATCH' : 'POST' }}"
                    id="yourmodule-entity-form"
                    action="{{ $submitUrl }}">
                @include('module-yourmodule::admin.entity.form.layout')

                <div class="form-group mt15">
                    {!! Form::button($item->exists ? trans('module-yourmodule::entity/form.btn_update') : trans('module-yourmodule::entity/form.btn_create'), [
                        'class' => 'btn bg-teal-400',
                        'type' => 'submit'
                    ]) !!}

                    <a href="{{ URL::previous() }}" class="btn btn-default">
                        {{ trans('module-yourmodule::entity/form.btn_cancel') }}
                    </a>
                </div>
            </v-form>
        </div>
    </yourmodule-entity-form>
@endsection

@push('style')
    @include('admin.vendor.form._styles')
@endpush

@push('script')
    @include('admin.vendor.form._scripts')
@endpush
```

With this view, we need to create a few partial views: `layout.blade.php` and `_tabs_detail.blade.php` in `admin/entity/form` directory.

`admin/entity/form/layout.blade.php`
```html
<?php /** @var \Modules\YourModule\Models\YourEntity $item */ ?>

@include('admin.vendor.form.panel_errors')

<v-tabs class="nav-tabs-custom" no-fade>
    {{-- General --}}
    <v-tab title="{{ trans('module-yourmodule::entity/form.tabs.details') }}"
           href="#general"
           active>
        @include('module-yourmodule::admin.entity.form._tab_details')
    </v-tab>
</v-tabs>
```

`admin/entity/form/_tabs_detail.blade.php`
```html
{{-- Name --}}
<v-form-group :required="true" :error="form.getError('name')">
    {!! Form::label('name', trans("module-yourmodule::entity/form.labels.name")) !!}
    {!! Form::text('name', null, [
        'class' => 'form-control maxlength',
        'maxlength' => '191',
        'v-model' => 'form.name',
    ]) !!}
</v-form-group>
```

In `admin/entity/form/_tabs_detail.blade.php` file, you can notice that we still use **Vue.js** library. 

> This example above is just **only for this demonstration** and `name` is just only an example column. This column will be stored 
> in our `YourEntity` model (and a database table).

Now, we can go back to `YourEntityController` and implement there the source code. This code can look like the one below:

```php 
...

use Modules\YourModule\Components\Forms\YourEntityForm;

...

    /**
     * GET: Create
     *
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function create()
    {
        $this->setTitleDescription(
            trans('module-yourmodule::entity/admin.pages.create.title'),
            trans('module-yourmodule::entity/admin.pages.create.description'));

        $item = new YourEntity();

        $form = new YourEntityForm($item);
        return $form->getView();
    }

...
```

**`YourEntityController::store()`**

Before the implementing of the `store` method, we need to create a request with validation rules. Let's make it:

```php
<?php

namespace Modules\YourModule\Http\Requests\Admin;

use App\Http\Requests\AbstractFormRequest;

class YourEntityRequest extends AbstractFormRequest
{
    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        $rules = [
            'name' => 'required|max:191'
        ];

        return $rules;
    }

    /**
     * Get the validation messages.
     *
     * @return array
     */
    public function messages()
    {
        return trans('module-yourmodule::entity/form.messages');
    }

    /**
     * Return input values.
     *
     * @return array
     */
    public function getValues(): array
    {
        $input = $this->all([
            'name'
        ]);

        return $input;
    }
}
```

It's just everything for our purpose. Now, continue in our `YourEntityController`.

```php
<?php
...

use Modules\YourModule\Http\Requests\Admin\YourEntityRequest;

...

    /**
     * POST: Store
     *
     * @param YourEntityRequest $request
     * @return \Illuminate\Http\RedirectResponse
     */
    public function store(YourEntityRequest $request)
    {
        YourEntity::query()->create($request->getValues());

        flash(trans('module-yourmodule::entity/general.notifications.created'), 'success');
        return $this->redirect(route('module.your_module.entity.index'));
    }

...
```

That's all about `store` method. If you need to implement more, you can do it what you want. You do not have any limits!

**`YourEntityController::edit()`**

For the next step, we will implement `edit` method. Because of a fact that we have prepared everything during the previous steps, we can
implement our code immediately inside the `YourEntityController`.

```php
<?php
...

    /**
     * GET: Edit
     *
     * @param YourEntity $item
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function edit(YourEntity $item)
    {
        $this->setTitleDescription(
            trans('module-yourmodule::entity/admin.pages.edit.title'),
            trans('module-yourmodule::entity/admin.pages.edit.description'));

        $form = new YourEntityForm($item);
        return $form->getView();
    }

...
```

For our `edit` method, that's all. Nothing more for doing here.

**`YourEntityController::update()`**

The `update` method will be almost the same like the `store` method. Just here we will work with an existed item.

```php 
<?php
...

    /**
     * PATCH: Update
     *
     * @param YourEntityRequest $request
     * @param YourEntity $item
     * @return \Illuminate\Http\RedirectResponse
     */
    public function update(YourEntityRequest $request, YourEntity $item)
    {
        $item->fill($request->getValues());
        $item->save();

        flash(trans('module-yourmodule::entity/general.notifications.updated'), 'success');
        return $this->redirect(route('module.your_module.entity.index'));
    }

...
```

**`YourEntityController::delete()`**

Now, this is the last step which we will do in `YourEntityController`.

```php 
<?php
...

    /**
     * DELETE: Delete
     *
     * @param YourEntity $item
     * @return \Illuminate\Http\JsonResponse|\Illuminate\Http\RedirectResponse
     * @throws \Exception
     */
    public function delete(YourEntity $item)
    {
        $item->delete();

        flash(trans('module-yourmodule::entity/general.notifications.deleted'), 'success');
        return $this->refresh();
    }

...
```

In the `YourEntityController` controller, we finished the work! **It's time to check** if everything is working well.

Log in to the Administration Panel of SIMPLO CMS and go to [Module Setting](../getting-started/setting.md#modules). There will appear
all available modules what you can install. If you did everything correctly, you will see in this list your new module as well. After
what you get find it, click on "Install" button.

Installation process does not take a long time and after that, in the admin menu there will appear the module's menu items, how you declared it
in your module's config file. That's great! 

Now you can click on a menu item of the module and try the following routes:

- `index` - admin/module/your-module
- `create` - admin/module/your-module/create
- `store` - admin/module/your-module/store
- `edit` - admin/module/your-module/edit/{item}
- `update` - admin/module/your-module/update/{item}
- `delete` - admin/module/your-module/delete/{item}

When everything is working after this testing, congratulations! You just created your first module with basic routes for SIMPLO CMS. If you want to know
more what SIMPLO CMS offers, continue in this guide.

> If you **want to create routes** for **a public web**, then use for this purpose directly your **theme working place**. For working with a theme,
> go to [Theme](../theme/general.md)

## Permission



## SEO, Open Graph

If you want to have a new entity with some editable SEO and Open Graph fields, then read this section.

For the first step, you will need to add columns to your database table. Create a database migration with a part of the following source code:

```php 
<?php
...

$table->string('seo_title')->nullable()->default(null);
$table->text('seo_description')->nullable()->default(null);
$table->boolean('seo_index')->default(true);
$table->boolean('seo_follow')->default(true);
$table->boolean('seo_sitemap')->default(true);

$table->text('open_graph')->nullable();

...
```

After that, **open your model** and **make the following changes**:

1. Use `App\Traits\OpenGraphTrait` trait inside your model.
2. Add SEO and Open Graph database columns to `$fillable` array for [Mass Assignment](https://laravel.com/docs/5.8/eloquent#mass-assignment).
3. It's a good way to add `seo_title` and `seo_description` columns to `$nullIfEmpty` array.
4. For correct casting, add `seo_index`, `seo_follow` and `seo_sitemap` to `$casts` array with `boolean` type value.

After this first steps, let's move to our example `YourEntityController` class and make the following changes:

```php
<?php

...

    /**
     * GET: Create
     *
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function create()
    {
        ...

        $item = new YourEntity();
        $item->forceFill([
            'seo_index' => true,
            'seo_follow' => true,
            'seo_sitemap' => true
        ]);

        ...
    }

...
```

In the `create` method, we added only `forceFill` method calling on `YourEntity` model because of default settings of SEO fields.
It's not necessary to set them on `true` value. For `YourEntityController`, it's everything.

Now we need to make changes in `YourEntityForm` class. There will just add `'seo_title', 'seo_description', 'seo_index', 'seo_follow', 'seo_sitemap', 'open_graph'`
columns in `$formAttributes` variable.

For the form, do not forget to update a source code inside the javascript file `entities.form.js`:

```js
...
import SeoInputs from '../../../../../../resources/assets/js/vue-components/form/seo-inputs';
import OpenGraphInputs from '../../../../../../resources/assets/js/vue-components/form/open-graph-inputs';

Vue.component('yourmodule-entity-form', {
    ...

    components: {
        'seo-inputs': SeoInputs,
        'open-graph-inputs': OpenGraphInputs,
    },

    ...
});
```

as well do not forget on blade views:

`admin/entity/form/layout.blade.php`
```html
...

<v-tabs class="nav-tabs-custom" no-fade>
    ...

    {{-- SEO --}}
    <v-tab title="{{ trans('module-yourmodule::entity/form.tabs.seo') }}"
           href="#seo"
    >
      @include('module-yourmodule::admin.entity.form._tab_seo')
    </v-tab>

    {{-- OpenGraph --}}
    <v-tab title="{{ trans('module-yourmodule::entity/form.tabs.og') }}"
           href="#open-graph"
    >
      @include('module-yourmodule::admin.entity.form._tab_og_tags')
    </v-tab>
</v-tabs>
```

for SEO and Open Graph form tabs, you need to create new files below:

`admin/entity/form/_tab_seo.blade.php`
```html
<seo-inputs :title-placeholder="form.name"
            :form="form"
            :trans="{{ \App\Helpers\Functions::combineTransToJson([
                'admin/general.seo', 'module-yourmodule::entity/form.seo_tab'
            ]) }}"
></seo-inputs>
```

`admin/entity/form/_tab_og_tags.blade.php`
```html
<open-graph-inputs :title-placeholder="form.name"
                   :form="form"
                   :trans="{{ json_encode(trans('admin/general.open_graph')) }}"
></open-graph-inputs>
```

For the validation of SEO and Open Graph fields, move to `YourEntityRequest` and make the following changes there:

```php
<?php

...

use App\Traits\Requests\ValidatesOpenGraphTrait;
use App\Traits\Requests\ValidatesSeoTrait;

class YourEntityRequest extends AbstractFormRequest
{
    use ValidatesSeoTrait,
        ValidatesOpenGraphTrait;

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        ...

        return $this->mergeRules(
            $rules, $this->getSeoRules(), $this->getOpenGraphRules()
        );
    }

    /**
     * Get the validation messages.
     *
     * @return array
     */
    public function messages()
    {
        return $this->mergeMessages(
            'module-yourmodule::entity/form.messages', $this->getSeoMessages(), $this->getOpenGraphMessages()
        );
    }

    /**
     * Return input values.
     *
     * @return array
     */
    public function getValues(): array
    {
        ...

        // SEO index
        $input['seo_index'] = $this->input('seo_index', false);

        // SEO follow
        $input['seo_follow'] = $this->input('seo_follow', false);

        // SEO sitemap
        $input['seo_sitemap'] = $this->input('seo_sitemap', false);

        ...
    }
}
...
```

It's done! Now you can go to the administration of your module's entity and check if there will be new SEO and Open Graph tabs.

## Multilingual

SIMPLO CMS can have a multilingual content. The multilingual technique offers you to have different entity records for 
all available languages. In the section, we will show you how you can do that.

First, it's necessary to add the following database column with a foreign key to `languages` table in your module entity's migration:

```php 
<?php
...

$table->unsignedInteger('language_id');
$table->foreign('language_id')->references('id')->on('languages')
    ->onUpdate('cascade')->onDelete('cascade');

...
```

Then, add using `App\Traits\HasLanguage` trait inside the `YourEntity` model. It's adding a few usable methods, mainly `language`
relation to the `App\Models\Web\Language`.

Next step for the multilingual implementation, open the `YourEntityTable` datatable and make the following changes:

```php
<?php
...

use App\Models\Web\Language;

...

class YourEntityTable extends AbstractDataTable
{
    /** @var \App\Models\Web\Language */
    private $language;

    ...

    /**
     * YourEntityTable constructor.
     *
     * @param \App\Models\Web\Language $language
     */
    public function __construct(Language $language, User $user)
    {
        $this->language = $language;
        $this->user = $user;

        parent::__construct();
    }

    ...

    /**
     * Get data query.
     *
     * @param \App\Structures\DataTable\FilterOptions $filterOptions
     * @return \Illuminate\Database\Query\Builder
     */
    protected function getDataQuery(FilterOptions $filterOptions)
    {
        $query = YourEntity::query()->whereLanguage($this->language);

        ...

        return $query;
    }

    ...
}
```

Mainly there is the `getDataQuery` method, where you need to add a where condition for the specific language loading of the items.

After that, go to `YourEntityController` and according to the changes above, we need to make a few changes there as well:

```php
<?php
...

    /**
     * POST: Store
     *
     * @param YourEntityRequest $request
     * @return \Illuminate\Http\RedirectResponse
     */
    public function store(YourEntityRequest $request)
    {
        $item = new YourEntity($request->getValues());

        $item->setLanguage($this->getLanguage());
        $item->save();

        ...
    }

...
```

That's everything for the implementation of the multilingual technique in SIMPLO CMS!

## Model URL

In this part of the complete guide, we will talk about **Model URL**. An entity can have SEO friendly url where users can visit
an item of some entity on a public web. Including the SEO friendly url, this model url feature makes for example automatically redirects when
the url was changed (because of change some part of url slug). Let's try to implement this feature for your entity!

How you know as well, the first step is just create a column for a database table.

```php 
...

$table->string('url');

...
```

It's nothing much, but it's everything. Just create an url string column for storing a generated url.

After this step, open the `YourEntity` model for doing the following changes:

1. Use `App\Traits\HasUrl` trait in your module. This trait adds few usable methods which manipulate with the url.
2. Implements `App\Models\Interfaces\UrlInterface` interface. It's important for recognizing by **SIMPLO CMS** if your model
has an url.
3. For [Mass Assignment](https://laravel.com/docs/5.8/eloquent#mass-assignment), add an `url` column in `$fillable` property.
4. This step is optional, but we recommend for doing that. Overwrite the `getUrlPrefix` method and defines your custom url prefix there. By default, it's
just only an url prefix with language code (if the model is a multilingual) and nothing more. It cannot be enough for all models what you will
create for the future as well.

Then, open the `YourEntityForm` and add `url` column inside the `$formAttributes` for fully loading it to the form view. With this step,
it's necessary to change the `admin/entity/form/_tab_details.blade.php` view. There, you can use the following changes:

```html
{{-- Nazev --}}
<v-form-group :required="true" :error="form.getError('name')">
    {!! Form::label('name', trans("module-yourmodule::entity/form.labels.name")) !!}
    {!! Form::text('name', null, [
        'class' => 'form-control maxlength',
        'maxlength' => '191',
        ':value' => 'form.name',
        '@change' => 'onNameChanged'
    ]) !!}
</v-form-group>

{{-- URL slug --}}
<v-form-group :required="true" :error="form.getError('url')">
    {!! Form::label('url', trans("module-yourmodule::entity/form.labels.url")) !!}
    {!! Form::text('url', null, [
        'class' => 'form-control maxlength',
        'maxlength' => '191',
        ':value' => 'form.url',
        '@change' => 'onUrlChanged'
    ]) !!}
</v-form-group>
```

For understandable the source code above, let's make the changes in `entities.form.js` as well:

```js
...

Vue.component('yourmodule-entity-form', {
    ...

    methods: {

        /**
         * Fired when name is changed.
         * @param {Event} $event
         */
        onNameChanged($event) {
            this.form.name = $event.target.value;

            if (this.form.url === null || !this.form.url.length) {
                this.form.url = this.form.name;
            }
        },

        /**
         * Fired when url is changed.
         * @param {Event} $event
         */
        onUrlChanged($event) {
            this.form.url = $event.target.value;
        }

    },

    watch: {
        'form.url'(newUrl) {
            this.form.url = Converter.removeDiacritics(newUrl);
        }
    }

});
```

It's understandable already more. For just an explanation, when an administrator will change the `name` input, 
the `url` input will be filled in by the `name` input and then with `Converter` library, this new `url` input value 
will be updated by `removeDiacritics` method. An url slug cannot contain any diacritic, so it's important for removing them.

> **You are free to implement** the `url` input **behavior** how you want. You **do not develop that the same**, but **remember** the `url` 
> slug **cannot contain any diacritics**. It means if administrators of your module entity will be able to fill in this field by themselves,
> it's **necessary** to make some corrections on a server side.

After the implementing an input `url` field, add a validation rule to the `YourEntityRequest`. Probably it will look the same like the request validation below:

```php
<?php
...
    $rules = [
        ...
        'url' => 'required|max:191'
        ...
    ];
...
```

Do not forget for adding the `url` column in the `getValues` method as well.

Now, there is only a last step for the correct implementation of Model URL. Everything's fine and we are able to fill in the url slug
with a request validation. But what about a storing this url slug to the database? In the `App\Traits\HasUrl` trait, there is the `registerUrlObserver`
method and this method registers for the model few events. These events serve for creating, updating and deleting the url slug of the entity
item. When we want to register this events for our `YourEntity` model, it's necessary to call it somewhere. The perfect place for doing this is
the `Modules\YourModule\Providers\ServiceProvider` provider.

```php
<?php
...

use Modules\YourModule\Models\YourEntity;

...

class ServiceProvider extends BaseProvider implements DeferrableProvider
{
    /**
     * Namespace of module resources and configuration.
     * @var string
     */
    protected $namespace = 'module-yourmodule';

    /**
     * Boot the application events.
     *
     * @return void
     */
    public function boot()
    {
        ...

        YourEntity::registerUrlObserver();
    }
```

After that, it's just everything for the completed using the basic SEO friendly url slugs! With the Model URL, you can 
set more things, and about them we will talk on the following rows.

The `App/Traits/HasUrl` trait offers more methods for manipulate with generated urls. You can overwrite the `getUrlSuperiorModels` method,
which provides an opportunity how define superior models. It means that when a system will generate a new url for your model item, these superior
models will be included for this generating (prefixes).

When you want to make an url prefix for the current model, then for this purpose it's possible to use `getUrlPrefix` what we have described 
above already.

If you do not want to have only single url then you can set `$hasSingleUrl` property on `true`. TODO!!!

For turning off automatically url sync, you can use `$manualUrlsSync` property and set it to `true`. TODO!!!

TODO !!! describe about redirecting and something like that.

## Media Library File

If you want to attach a file from [Media Library](../core/media-library.md), read this section of this complete guide.

For the first step of course, we will need to increase a database table using a migration with `media` macro:

```php 
<?php
...

$table->media('file_id');

...
```

You do not keep the `file_id` name for your media library file column. 

> If **you are interested in** about the `media` macro, you can check the source code inside the `App\Providers\MediaLibraryProvider` service provider.

After running this database migration, you will be able to store a file id to your database. 

If you wish to attach only image file from Media Library, it's necessary to use `App\Traits\MediaImageTrait` trait in your
`YourEntity` model. After that, add `file_id` column to `$fillable` array for [Mass Assignment](https://laravel.com/docs/5.8/eloquent#mass-assignment) and
for better work with empty value, add this column to `$nullIfEmpty` as well. With that, make a correct integer casting for the `file_id` column using `$casts` array.
How a last step in `YourEntity` model, add `$this->belongsTo(File::class, 'file_id');` relationship source code. In the `YourEntity` model, just we are done.

For the next step, we must add `file_id` column to `$formAttributes` variable in `YourEntityForm`. With this update, it's necessary to increase the `admin/entity/form/_tab_details.blade.php` file
about `file_id` input for selecting a file.

```html
...

{{-- Media Library File --}}
<v-form-group :has-error="form.hasError('file_id')">
  {!! Form::label('file_id', trans("module-yourmodule::entity/form.labels.file_id")) !!}

  <media-library-file-selector :image="true"
                               name="file_id"
                               :error="form.getError('file_id')"
                               v-model="form.file_id"
  ></media-library-file-selector>
</v-form-group>

...
```

If the file from Media Library will not be only image file, then you must remove `:image="true"` from the source code above.

Now, it's only last step in front of us. Open `YourEntityRequest` and make the following changes:

1. If your Media Library file will be only image, then add `'file_id' => ['nullable', new MediaImageRule]` rule validation. If it's not just an image file, then
do not use `App\Rules\MediaImageRule`. For sure, you can change `nullable` rule to `required` if you want that a user must select this file.
2. Add your Media Library file column in the `getValues` method.

After the steps above, you can try if everything is fine.

#### How To Use

For loading a collection of our models with files, we can use `with` method on our model class:

```php
<?php

$items = YourModel::with('file')->get();
```

Then in some cycle (foreach), we will print each file using `makeLink` source code. If our file is only image, then
we can use for better manipulation with this image the `makeImageLink` method directly on our model item:

```php
<?php

$item->makeImageLink('image');
```

The `makeImageLink` method returns the `App\Services\MediaLibrary\ImageBuilder` instance which provides a lot of useful methods for
manipulation with this image:

**`resize(int $width, int $height): ImageBuilder`**
Resizes a current image based on a given width and/or a height.

**`fit(int $width, int $height): ImageBuilder`**
Combine cropping and resizing to a format image by a smart way. The method will find the best fitting aspect 
ratio of a given width and height on the current image automatically, cut it out and resize it to the given dimension.

**`crop(int $width, int $height): ImageBuilder`**
Cut out a rectangular part of the current image with given width and height.

**`greyscale(): ImageBuilder`**
Turns image into a greyscale version.

**`fitCanvas(int $width, int $height, ?string $color = null): ImageBuilder`**
Resize the boundaries of the current image to a given width and a height. You can also pass a background 
color for the rest area of the image.

**`allowedFormats(array $formats): ImageBuilder`**
Returns image in one of given formats.

**`formatPNG(): ImageBuilder`**
Returns an image in PNG format.

How you can notice, all these methods above returns the `App\Services\MediaLibrary\ImageBuilder` instance (by `$this` pointer).
It means that you can make a chain for applying more operations. 

All these operations are specified as parameters inside a query string of the image url link. By these parameters, a server generates
a thumbnail of the image, which will be optimized, stored in a cache memory and returns as a response.

For returning an url link for this image with all applied operations, call the `getUrl()` method on the instance.
Including the `getUrl()` method, you can use `setFallbackUrl(string $imageUrl)` as well. For this method, you must define
the `$imageUrl` parameter, what represents a fallback image url. This fallback image url will be returned by the `getUrl()` method, when
the current image does not exist or it is not a valid image.

> It's **a good way to use for all images** selected from Media Library just the `makeImageLink` method. Why? The reason is simple. 
> All files (and for sure images) in Media Library **can be removed** by a user through this library or **can be renamed** with non valid 
> image file as well. The `makeImageLink` method will always return an url link to a valid image. If this image will not exist or this image
> will not be a valid image, then this method will return an url link to a placeholder.
>
> **Great advantage** about this attitude is also a fact that the `makeImageLink` method **always returns the specific url link to this placeholder.**
> It means that if this `App\Services\MediaLibrary\ImageBuilder` instance will start to return a valid image after the placeholder later, this change will
> be applied immediately (because this image will have a different url address than this placeholder).

#### Example

```php
<?php

// returns a black white image with size 200x200
$imageUrl = $item->makeImageLink('image')->fitCanvas(200, 200)->greyscale()->getUrl();
```

```php
<?php

// example with set a fallback url
$imageUrl = $item->makeImageLink('image')->setFallbackUrl('https://i.imgur.com/ycrW4Pv.png')->resize(400, 300)->getUrl();
```

## Publishing Mechanism

In this section, we will talk about publishing mechanism. It is mechanism which for developers offers to make some entity with
publish stating. It means that through an administration panel, administrators can published or unpublished items of the entity.

How in the previous things, for the first step here we need to make some database migrations as well. For our purpose, we need to add
the following columns for our entity:

```php
<?php
...

$table->unsignedTinyInteger('state')
                ->default(\App\Structures\Enums\PublishingStateEnum::PUBLISHED)
                ->index();
$table->dateTime('publish_at');
$table->dateTime('unpublish_at')->nullable()->default(null);

...
```

After running this new database migration, move to your entity model and make the following changes:

1. Use `App\Traits\PlannedPublishingTrait`, which attach a few useful methods for your entity model.
2. For better identification of your publishable model, implements `App\Contracts\PublishableModelInterface` interface.
3. Add `publish_at`, `unpublish_at` and `state` to `$fillable` array.
4. Add `publish_at` and `unpublish_at` to `$dates` because of correct and more comfortable working with these dates.
5. For correct casting, you can add `state` to `$casts` with `int` value.
6. Add the following methods in your entity model:

```php
<?php
...

use App\Structures\Enums\PublishingStateEnum;

...

    /**
     * Check if model is public.
     *
     * @return bool
     */
    public function isPublic(): bool
    {
        return $this->state === PublishingStateEnum::PUBLISHED && $this->isInPublicPeriod();
    }

    /**
     * Select only published articles
     *
     * @param \Illuminate\Database\Eloquent\Builder|self $query
     */
    public function scopePublished($query)
    {
        $query->publishedByDate()->where('state', PublishingStateEnum::PUBLISHED);
    }

...
```

After this step where you updated your entity model, open `YourEntityForm` and make the following changes inside the
`getViewData` method:

```php
<?php
...

use App\Structures\Enums\PublishingStateEnum;

...
    /**
     * Get view data.
     *
     * @return array
     */
    public function getViewData(): array
    {
        $formAttributes = [
            ...
            'state', 'publish_at_date', 'publish_at_time', 'unpublish_at_date', 'unpublish_at_time'
        ];

        return [
            ...
            'publishingStates' => PublishingStateEnum::toJson(),
        ];
    }
...
```

With `YourEntityForm` you need to change `entities.form.js` file:

```js
...
import PublishingStateInputs from '../../../../../../resources/assets/js/vue-components/form/publishing-state-inputs';

Vue.component('yourentity-form', {
    data() {
        return {
            form: new Form({
                ...
                set_unpublish_at: this.entity.unpublish_at_date !== null
            }).addDataCollector(this.getFormData),
            ...
            showPublishingInputs: true
        };
    },
    
    ...

    components: {
        ...
        'publishing-state-inputs': PublishingStateInputs,
    },
    
    ...

    methods: {
        tabActivated (tab) {
            // When first tab (index zero) is activated
            this.showPublishingInputs = tab === 0;
        },

        ...
    },

    ...
});
```

and `admin/entity/form/layout.blade.php` with `admin/entity/form/_tab_state.blade.php`:

```html
<?php /** @var \Modules\YourModule\Models\YourEntity $entity */ ?>

@include('admin.vendor.form.panel_errors')

<div class="row">
  <div class="no-padding-right" :class="[showPublishingInputs ? 'col-md-9' : 'col-xs-12']">
    <v-tabs class="nav-tabs-custom" @input="tabActivated" no-fade>
        {{-- General --}}
        <v-tab title="{{ trans('module-yourmodule::entity/form.tabs.details') }}"
               href="#general"
               active
        >
            @include('module-yourmodule::admin.entity.form._tab_details')
        </v-tab>

        ...
    </v-tabs>
  </div>
  <div class="col-md-3 mt-20" v-show="showPublishingInputs">
    <div class="panel panel-default mt-20">
      <div class="panel-heading text-uppercase">{{ trans('module-yourmodule::entity/form.tabs.state') }}</div>
      <div class="panel-body">
        @include('module-yourmodule::admin.entity.form._tab_state')
      </div>
    </div>
  </div>
</div>
```

```html
<publishing-state-inputs :publishing-states="{{ $publishingStates }}"
                         :form="form"
                         :trans="{{ json_encode(trans('admin/general.publishing_states_component')) }}"
></publishing-state-inputs>
```

How you can notice, for printing publishing state inputs we use Vue.js component. Now it's time to make a few 
changes inside the `YourEntityRequest` request:

```php
<?php
...

use App\Traits\Requests\ReceivesPlannedPublishingTrait;

...

class YourEntityRequest extends AbstractFormRequest
{
    use ReceivesPlannedPublishingTrait,
        ...;

    /**
     * Return input values.
     *
     * @return array
     */
    public function getValues(): array
    {
        $input = $this->all([
            ...
        ]);

        ...

        $input['publish_at'] = $this->getPublishAt();
        $input['unpublish_at'] = $this->get('set_unpublish_at') ? $this->getUnpublishAt() : null;

        ...

        return $input;
    }

    ...
}
```

That's everything and we can make finally changes in `YourEntityController`:

```php
<?php

...

use App\Structures\Enums\PublishingStateEnum;

...

class YourEntityController extends AdminController
{
    ...

    /**
     * GET: Create
     *
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function create()
    {
        ...

        $entity = new YourEntity();
        $entity->forceFill([
            'publish_at' => \Carbon\Carbon::now(),
            'state' => PublishingStateEnum::PUBLISHED,
            ...
        ]);

        ...
    }

    ...
}
```

> You do not need to keep this `YourEntityController` source code when you do not want to have these default values. It's up to you
> how you will change them.

## Grid Editor Implementation

This section will give you information about that how you can use Grid Editor content for your entity. It's a good way when you need to
have editable layout content.

> If you want to get more information about **Grid Editor**, visit [Grid Editor page](../core/grid-editor.md) first.

How in general, for the first step we will increase database about a few new tables. For your entity model, which uses Grid Editor, it's not
necessary to do anything special. But it's important to make a database migration for creating a table for Grid Editor content:

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateYourEntityContentsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('your_entity_contents', function (Blueprint $table) {
            $table->increments('id');

            $table->unsignedInteger('your_entity_id');
            $table->foreign('your_entity_id')->references('id')->on('your_entity_table')
                ->onUpdate('cascade')->onDelete('cascade');

            $table->text('content')->nullable();
            $table->boolean('is_active'); // for versioned content

            $table->unsignedInteger('author_user_id')->nullable()->default(null); // for versioned content
            $table->foreign('author_user_id')->references('id')->on('users')
                  ->onUpdate('cascade')->onDelete('set null');

            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('your_entity_contents');
    }
}
```

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateYourEntitySectionsClosuresTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('your_entity_contents_closures', function (Blueprint $table) {
            $table->unsignedInteger('ancestor_id');
            $table->foreign('ancestor_id')
                ->references('id')->on('your_entity_contents')
                ->onDelete('cascade');

            $table->unsignedInteger('descendant_id');
            $table->foreign('descendant_id')
                ->references('id')->on('your_entity_contents')
                ->onDelete('cascade');

            $table->unsignedSmallInteger('depth');

            $table->primary(['ancestor_id', 'descendant_id']);
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('your_entity_contents_closures');
    }
}
```

For the content database table, there is mandatory to have `content` column and a foreign id column to your entity item. If you 
want to have this content versioned, then you need to specify optional `is_active` and `author_user_id` columns.

After running these database migrations, move to your entity model and make the following changes:

1. Your entity model must extend from `App\Structures\ClosureTable\HierarchicModel`.
2. Implements `App\Models\Interfaces\UsesGridEditor`.
3. Use `App\Traits\GridEditor\GridEditorModel` for easier implementation.
4. You can set `protected $useGridEditorVersions` to `true` value for Grid Editor versions. If you will turn on it, then
your entity items will have versioning of all Grid Editor contents. Then you will be able to go back to your older changes of
these contents. Don't forget that a database content table must consist `is_active` and `author_user_id` columns.
5. Add another source code below for `YourEntity` model:

```php 
<?php
...

    /**
     * Contents - versions.
     *
     * @return \Illuminate\Database\Eloquent\Relations\HasMany
     */
    public function contents(): \Illuminate\Database\Eloquent\Relations\HasMany
    {
        return $this->hasMany(Content::class, 'your_entity_id');
    }

...

    /**
     * Get active content.
     *
     * @return string
     */
    public function getContent(): string
    {
        return $this->getActiveContent()->getHtml([
            'language' => $this->language
        ]);
    }

...
```

If you want to make your Grid Editor content searchable, make your `search` method like this below:

```php 
<?php

...

    /**
     * Search in contents.
     *
     * @param string $term
     * @param \App\Models\Web\Language $language
     * @return \Illuminate\Support\Collection
     */
    public static function search(string $term, Language $language): Collection
    {
        if (!strlen($term)) {
            return collect([]);
        }

        $items = self::whereLanguage($language)
            ->with([
                'contents' => function (HasMany $query): void {
                    $query->where('is_active', true);
                },
            ])
            ->get();

        return self::searchGridContent($term, $items, $language, function (self $item) use ($term) {
            return mb_stripos($item->title, $term) !== false || mb_stripos($item->seo_title, $term) !== false;
        });
    }

...
```

For sure, it's up how you implemented your entity (maybe your entity doesn't use `title` and `seo_title` columns. 
But you must call the `searchGridContent` method and return her result.

After making changes inside your entity module using Grid Editor content, then it's needed to define this content model as well:

```php
<?php

namespace Modules\YourModule\Models;

use App\Models\User;
use App\Traits\AdvancedEloquentTrait;
use Illuminate\Database\Eloquent\Model;
use App\Traits\GridEditor\GridEditorContent;
use App\Models\Interfaces\IsGridEditorContent;

/**
 * Class Content
 * @package Modules\YourModule\Models
 * @author SIMPLO, s.r.o.
 * @copyright SIMPLO, s.r.o.
 *
 * @property int your_entity_id
 * @property string content
 * @property bool is_active
 * @property int|null author_user_id
 * @property \Carbon\Carbon created_at
 * @property \Carbon\Carbon updated_at
 *
 * @property-read \App\Models\User author
 */
class Content extends Model implements IsGridEditorContent
{
    use AdvancedEloquentTrait, GridEditorContent;

    /**
     * Model table.
     *
     * @var string
     */
    protected $table = 'your_entity_contents';

    /**
     * Mass assignable attributes.
     *
     * @var array
     */
    protected $fillable = ['content', 'is_active'];

    /**
     * Author of content version.
     * @return \Illuminate\Database\Eloquent\Relations\HasOne
     */
    public function author()
    {
        return $this->hasOne(User::class, 'id', 'author_user_id');
    }

    /**
     * Set current content active.
     */
    public function setActive()
    {
        if ($this->is_active) {
            return;
        }

        self::where('your_entity_id', $this->your_entity_id)
            ->where('id', '<>', $this->id)
            ->update([
                'is_active' => false
            ]);

        $this->update([
            'is_active' => true
        ]);
    }
}
```

Now, the next step is up on your decision about versioning Grid Editor content. If you want to have versioning, then you need to
add the following route in `Http/routes.php` of your module:

```php
<?php
...
    Route::get('{yourEntity}/switch-version/{contentId?}', [
        'as' => 'switch_version',
        'uses' => 'YourEntityController@switchVersion',
    ]);
...
```

After that, we can start to implement a form class.

```php
<?php

namespace Modules\YourModule\Components\Forms;

use App\Components\Forms\Templates\AbstractFormWithGridEditor;
...

class YourEntityForm extends AbstractFormWithGridEditor
{
    ...

    /**
     * Use grid editor versions?
     *
     * @var boolean
     */
    protected $useVersions = true;

    ...

    /**
     * YourEntity form.
     *
     * ...
     * @throws \Exception
     */
    public function __construct(...)
    {
        parent::__construct();
        
        ...

        $this->addGridEditorScriptsAndStyle();

        ...
    }

    /**
     * Get view data.
     *
     * @return array
     */
    public function getViewData(): array
    {
        ...

        $data = [
            ...
        ];

        return $this->getGridEditorData($data);
    }

    /**
     * Get content of the grid editor.
     *
     * @return \App\Models\Interfaces\IsGridEditorContent
     */
    protected function getGridEditorContent(): \App\Models\Interfaces\IsGridEditorContent
    {
        return $this->item->getActiveContent() ?: new YourEntityContent();
    }

    /**
     * Get versions of the grid editor content.
     *
     * @return array
     */
    protected function getGridEditorVersions(): array
    {
        return $this->item->getGridEditorVersions();
    }

    /**
     * Get version switch url
     *
     * @return string
     */
    protected function getGridEditorVersionSwitchUrl(): string
    {
        // - for no existed entity item, we can return just only an empty string because probably in all cases
        // it will be possible to switch versions only for an existed item
        if (!$this->item->exists) {
            return "";
        }

        return route('module.yourmodule.entity.switch_version', [$this->item]);
    }
}
```

When you will look at that source code above, you can notice that `YourEntityForm` extends `App\Components\Forms\Templates\AbstractFormWithGridEditor`.
This extended class provides a few useful methods for working with data variables about Grid Editor and transmits them to a form view. All data variables
for Grid Editor have a prefix `$_GE_` and developers can overwrite them in the `getViewData` method.
Except of this change, here is `$useVersions` set on `true` for Grid Editor versioning of your entity item content. If you don't use a versioning, don't
overwrite this instance variable at all.

With `YourEntityForm`, you need to increase the `admin/entity/form/layout.blade.php` view as well:

```html
<?php /** @var \Modules\YourModule\Models\YourEntity $item */ ?>

@include('admin.vendor.form.panel_errors')

<v-tabs class="nav-tabs-custom" no-fade>
    {{-- General --}}
    <v-tab title="{{ trans('module-yourmodule::entity/form.tabs.details') }}"
           href="#general"
           active
    >
        @include('module-yourmodule::admin.entity.form._tab_details')
    </v-tab>

    {{-- Grid --}}
    <v-tab title="{{ trans('module-yourmodule::entity/form.tabs.grid') }}"
           href="#grid"
    >
        @include('module-yourmodule::admin.entity.form._tab_grid')
    </v-tab>

    ...
</v-tabs>
```

In `admin/entity/form/_tab_grid.blade.php`, there will be the following source code below:

```php 
@include('admin.grideditor._input')
```

It's really simple and nothing more is needed.

With `YourEntityForm` you need to change `entities.form.js` file:

```js
...

Vue.component('yourmodule-entity-form', {

    ...

    methods: {
        ...

        getFormData() {
            const additionalData = this.$refs.gridEditor.getFormData();

            ...

            return additionalData;
        }
    },

    ...

});
```

Now add the `getGridEditorContent` method in the `YourEntityRequest` request:

```php
<?php
...

    /**
     * Get content.
     *
     * @return array|null
     */
    public function getGridEditorContent(): ?array
    {
        return $this->input('content');
    }

...
```

It's not necessary, but it's good for better manipulation with the request instance. After that implementation above, 
we can make finally changes in `YourEntityController`:

```php
<?php

...

use App\Exceptions\GridEditorException;

...

class YourEntityController extends AdminController
{
    /**
     * SectionsController constructor.
     */
    public function __construct()
    {
        parent::__construct();

        ...

        $this->middleware('permission:module_organisationunit-edit')
            ->only(['edit', 'update', 'switchVersion']);

        ...
    }

    ...

    /**
     * POST: Store
     *
     * @param YourEntityRequest $request
     * @return \Illuminate\Http\RedirectResponse
     * @throws \App\Exceptions\GridEditorException
     */
    public function store(YourEntityRequest $request)
    {
        ...

        // Save content.
        try {
            $item->createNewVersionIfChanged($request->getGridEditorContent());
        } catch (GridEditorException $e) {
            $section->forceDelete();
            throw $e;
        }

        flash(trans('module-yourmodule::entity/general.notifications.created'), 'success');
        return $this->redirect(route('module.yourmodule.entity.edit', [$item->id]));
    }

    ...

    /**
     * PATCH: Update
     *
     * @param YourEntityRequest $request
     * @param YourEntity $item
     * @return \Illuminate\Http\RedirectResponse
     * @throws \App\Exceptions\GridEditorException
     */
    public function update(YourEntityRequest $request, YourEntity $item)
    {
        $item->fill($request->getValues());
        $item->save();

        // Set active content before content is examined for changes.
        /** @var Content $activeContent */
        $activeContent = $item->contents()->find($request->input('active_content_id'));
        if ($activeContent) {
            $activeContent->setActive();
        }

        // Examine content for changes and save it with modules.
        /** @var Content $newVersion */
        $newVersion = $item->createNewVersionIfChanged($request->getGridEditorContent());

        $content = $newVersion ?? $activeContent;

        $successMessage = trans('module-yourmodule::entity/general.notifications.updated');

        if ($request->get('prevent_redirect')) {
            return response()->json([
                'message' => $successMessage,
                'newContent' => $content && $content->wasRecentlyCreated ? $content->getRaw() : null,
                'versions' => $content && $content->wasRecentlyCreated ? $item->getGridEditorVersions() : null,
                'url' => $item->url
            ]);
        }

        flash($successMessage, 'success');
        return $this->redirect(route('module.organisationunit.entity.edit', [$item->id]));
    }

    ...

    /**
     * Switch active content.
     *
     * @param YourEntity $item
     * @param int|null $versionId
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function switchVersion(YourEntity $item, $versionId = null): JsonResponse
    {
        if (!$versionId) {
            abort(404);
        }

        /** @var Content $content */
        $content = $item->contents()->find($versionId);

        if (!$content) {
            abort(404);
        }

        return response()->json([
            'content' => $content->getRaw()
        ]);
    }
}
```

That's all. Now you can try this implemented Grid Editor content!

## Photogallery Entity

SIMPLO CMS offers you to make a wonderful photogallery entity. Everything, what you will need, is a part of SIMPLO CMS.

For the first step, it's necessary to prepare database tables. For a purpose of the photogallery, we need to have minimum 2 database
tables - `photogalleries` and `photogallery_photos`.

```php 
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreatePhotogalleriesTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('photogalleries', function (Blueprint $table) {
            $table->increments('id');

            $table->string('title');
            $table->string('url');

            $table->text('text')->nullable()->default(null);

            $table->string('seo_title')->nullable()->default(null);
            $table->text('seo_description')->nullable()->default(null);

            $table->boolean('seo_index')->default(true);
            $table->boolean('seo_follow')->default(true);
            $table->boolean('seo_sitemap')->default(true);

            $table->text('open_graph')->nullable();

            $table->timestamps();
        });

        Schema::create('photogallery_photos', function (Blueprint $table) {
            $table->increments('id');

            $table->unsignedInteger('photogallery_id')->nullable()->default(null);
            $table->foreign('photogallery_id')->references('id')->on('photogalleries')
                ->onDelete('cascade');

            $table->string('title')->nullable()->default(null);
            $table->string('author')->nullable()->default(null);
            $table->media('image_id');

            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('photogallery_photos');
        Schema::dropIfExists('photogalleries');
    }
}
```

You do not keep the source code strictly. If you are not comfortable to have [SEO, Open Graph](modules/complete-guide.md#seo-open-graph) 
or [Model URL](modules/complete-guide#model-url) for your photogallery, you do not need to have it there. Just remove these database 
migration columns and make your photogallery without them.

After running these database migrations, make two models:

```php
<?php

namespace Modules\YourModule\Models\Photogallery;

use App\Contracts\PhotogalleryInterface;
use App\Contracts\ViewableModelInterface;
use App\Services\FrontWeb\FrontWebData;
use App\Services\FrontWeb\FrontWebService;
use App\Services\FrontWebTools\ToolbarOptions;
use App\Traits\AdvancedEloquentTrait;
use App\Traits\OpenGraphTrait;
use App\Traits\PhotogalleryTrait;
use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsTo;
use Illuminate\Database\Eloquent\Relations\HasMany;
use Illuminate\Database\Eloquent\Relations\HasOne;
use Illuminate\Support\Collection;

/**
 * Class Photogallery
 * @package \Modules\YourModule\Models\Photogallery\Photogallery
 * @author SIMPLO, s.r.o.
 * @copyright SIMPLO, s.r.o.
 *
 * @property int views
 * @property string title
 * @property string url
 * @property string|null text
 * @property string|null seo_title
 * @property string|null seo_description
 * @property bool seo_index
 * @property bool seo_follow
 * @property bool seo_sitemap
 * @property \Carbon\Carbon created_at
 * @property \Carbon\Carbon updated_at
 */
class Photogallery extends Model implements
    PhotogalleryInterface,
    ViewableModelInterface
{
    use AdvancedEloquentTrait,
        PhotogalleryTrait,
        OpenGraphTrait;

    /**
     * The database table used by the model.
     *
     * @var string
     */
    protected $table = 'photogalleries';

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'title', 'url', 'text', 'open_graph',
        'seo_title', 'seo_description', 'seo_index', 'seo_follow', 'seo_sitemap'
    ];

    /**
     * The attributes that are set to null when the value is empty
     *
     * @var array
     */
    protected $nullIfEmpty = [
        'text', 'seo_title', 'seo_description',
    ];

    /**
     * The attributes that should be cast to native types.
     *
     * @var array
     */
    protected $casts = [
        'seo_index' => 'boolean',
        'seo_follow' => 'boolean',
        'seo_sitemap' => 'boolean',
    ];

    /**
     * Photos.
     *
     * @return \Illuminate\Database\Eloquent\Relations\HasMany
     */
    public function photos(): HasMany
    {
        return $this->hasManyPhotos('photogallery_photos', 'photogallery_id');
    }

    /**
     * Get view data of the model.
     *
     * @param \App\Services\FrontWeb\FrontWebData $data
     * @return \App\Services\FrontWeb\FrontWebData
     */
    public function setFrontWebData(FrontWebData $data): FrontWebData
    {
        $data->setTitle($this->seo_title ?: $this->title);
        $data->setDescription($this->seo_description);
        $data->setIndex($this->seo_index);
        $data->setFollow($this->seo_follow);
        $data->setOpenGraphTags([
            'og:title' => $this->open_graph->get('title'),
            'og:description' => $this->open_graph->get('description'),
            'og:type' => $this->open_graph->get('type', 'website'),
            'og:url' => $this->open_graph->get('url'),
            'og:image' => $this->open_graph->hasImage() ? $this->open_graph->makeImageLink()->getUrl() : null
        ]);

        return $data;
    }

    /**
     * Set options for front-web toolbar.
     *
     * @param \App\Services\FrontWebTools\ToolbarOptions $options
     */
    public function setFrontWebToolbarOptions(ToolbarOptions $options): void
    {
        $options->addControl(
            trans('module-yourmodule::photogallery/admin.frontweb_toolbar.btn_edit'),
            route('module.your_module.photogallery.edit', [$this->id]),
            'edit'
        );
    }

    /**
     * Search photogalleries for given term.
     *
     * @param string $term
     * @return \Illuminate\Support\Collection
     */
    public static function search(string $term): Collection
    {
        if (!strlen($term)) {
            return collect([]);
        }

        return self::query()
            ->get()
            ->filter(function (self $photogallery) use ($term) {
                return mb_stripos($photogallery->title, $term) !== false ||
                    mb_stripos($photogallery->seo_title, $term) !== false ||
                    mb_stripos(strip_tags($photogallery->text), $term) !== false;
            });
    }

}
```

```php
<?php

namespace Modules\YourEntity\Models\Photogallery;

use App\Models\Media\File;
use App\Traits\AdvancedEloquentTrait;
use App\Traits\MediaImageTrait;
use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsTo;

/**
 * Class Photo
 * @package \Modules\YourEntity\Models\Photogallery\Photo
 * @author SIMPLO, s.r.o.
 * @copyright SIMPLO, s.r.o.
 *
 * @property string|null title
 * @property int image_id
 *
 * @property-read \App\Models\Media\File image
 */
class Photo extends Model
{
    use AdvancedEloquentTrait, 
        MediaImageTrait;

    /**
     * The database table used by the model.
     *
     * @var string
     */
    protected $table = 'photogallery_photos';

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = ['title', 'image_id'];

    /**
     * The attributes that should be cast to native types.
     *
     * @var array
     */
    protected $casts = [
        'image_id' => 'int',
    ];

    /**
     * Relation to image file.
     *
     * @return \Illuminate\Database\Eloquent\Relations\BelongsTo
     */
    public function image(): BelongsTo
    {
        return $this->belongsTo(File::class, 'image_id');
    }

    /**
     * Convert photo to array for photogallery.
     *
     * @return array
     */
    public function toArray()
    {
        return [
            'id' => $this->getKey(),
            'title' => $this->title,
            'image' => optional($this->image)->toArray() ?? []
        ];
    }

    /**
     * Get image url.
     *
     * @return string
     */
    public function getUrl(): string
    {
        return $this->makeImageLink('image')->getUrl();
    }

    /**
     * Create a new instance of the given model.
     *
     * @param  array  $attributes
     * @param  bool  $exists
     * @return Photo
     */
    public function newInstance($attributes = [], $exists = false)
    {
        /** @var Photo $model */
        $model = parent::newInstance($attributes, $exists);
        $model->setTable($this->getTable());

        return $model;
    }
}
```

After creating all necessary models, open the `Http/routes.php` file in your module and make few routes:

```php
<?php

Route::group([
    'middleware' => 'web',
    'prefix' => 'admin/module/your-module',
    'namespace' => 'Modules\YourModule\Http\Controllers\Admin',
    'as' => 'module.your_module.'
], function () {

    Route::group([
        'prefix' => 'photogallery',
        'as' => 'photogallery.'
    ], function () {

        Route::get('/', [
            'as' => 'index',
            'uses' => 'PhotogalleriesController@index',
        ]);

        Route::get('create', [
            'as' => 'create',
            'uses' => 'PhotogalleriesController@create',
        ]);

        Route::post('upload-photo/{photogallery?}', [
            'as' => 'upload_photo',
            'uses' => 'PhotogalleriesController@uploadPhoto',
        ]);

        Route::post('store', [
            'as' => 'store',
            'uses' => 'PhotogalleriesController@store',
        ]);

        Route::get('edit/{photogallery}', [
            'as' => 'edit',
            'uses' => 'PhotogalleriesController@edit',
        ]);

        Route::patch('update/{photogallery}', [
            'as' => 'update',
            'uses' => 'PhotogalleriesController@update',
        ]);

        Route::delete('delete/{photogallery}', [
            'as' => 'delete',
            'uses' => 'PhotogalleriesController@delete',
        ]);

        // Photo

        Route::get('photo/list/{photogallery?}', [
            'as' => 'photo.list',
            'uses' => 'PhotogalleriesController@photoList',
        ]);

        Route::post('photo/update/{photogallery?}', [
            'as' => 'photo.update',
            'uses' => 'PhotogalleriesController@updatePhoto',
        ]);

        Route::delete('photo/{photo}/delete', [
            'as' => 'photo.delete',
            'uses' => 'PhotogalleriesController@deletePhoto',
        ]);

    });

});
```

Now, we need to make a few classes - `PhotogalleryTable`, `PhotogalleryForm` and `PhotogalleryRequest`:

```php
<?php

namespace Modules\YourModule\Components\DataTables;

use App\Components\DataTables\AbstractDataTable;
use App\Models\User;
use App\Structures\DataTable\FilterOptions;
use Illuminate\Support\Collection;
use Modules\YourModule\Models\Photogallery\Photogallery;

class PhotogalleryTable extends AbstractDataTable
{
    /** @var \App\Models\User */
    private $user;

    /**
     * PhotogalleryTable constructor.
     */
    public function __construct(User $user)
    {
        $this->user = $user;

        parent::__construct();
    }

    /**
     * Initialize datatable.
     */
    protected function initialize(): void
    {
        $this->createColumn('title', trans('module-yourmodule::photogallery/general.table.columns.title'))
            ->makeSortable();

        $this->setActionsVisibility($this->user->can(['module_yourmodule-edit', 'module_yourmodule-delete']));
    }

    /**
     * Get data query.
     *
     * @param \App\Structures\DataTable\FilterOptions $filterOptions
     * @return \Illuminate\Database\Query\Builder
     */
    protected function getDataQuery(FilterOptions $filterOptions)
    {
        $query = Photogallery::query();

        // check active a column for sorting
        switch ($filterOptions->getSortingColumn()) {
            case 'title':
                $filterOptions->sort();
                break;
        }

        $filterOptions->searchOnColumns('title');
        return $query;
    }

    /**
     * Fill table with fetched data.
     *
     * @param \Illuminate\Support\Collection|Photogallery[] $photogalleries
     * @return void
     */
    protected function fill(Collection $photogalleries): void
    {
        foreach ($photogalleries as $photogallery) {
            $row = $this->addRow($photogallery->getKey());

            $routeParams = [
                'photogallery' => $photogallery->id
            ];

            // Edit
            if ($this->user->can('module_yourmodule-edit')) {
                $row->setDoubleClickAction(route('module.your_module.photogallery.edit', $routeParams));
                $row->addControl(
                    trans('module-yourmodule::photogallery/general.table.btn_edit'),
                    route('module.your_module.photogallery.edit', $routeParams),
                    'pencil-square-o'
                );
            }

            // Delete
            if ($this->user->can('module_yourmodule-delete')) {
                $row->addControl(
                    trans('module-yourmodule::photogallery/general.table.btn_delete'),
                    route('module.your_module.photogallery.delete', [$photogallery->getKey()]),
                    'trash'
                )->setDelete(trans('module-yourmodule::entity/general.confirm_delete'));
            }

            // Columns
            $row->addColumn('title', $photogallery->title);
        }
    }
}
```

In `PhotogalleryTable`, there is nothing what you would not know yet in this part of the complete guide. Let's continue and
create `admin/photogallery/index.blade.php`:

```html
<?php /** @var \Modules\YourModule\Components\DataTables\PhotogalleryTable $table */ ?>
@extends('admin.layouts.master')

@section('content')
  <v-datatable :table="{{ $table->toJson() }}"></v-datatable>

  @permission('module_yourmodule-create')
    <a href="{{ route('module.your_module.photogallery.create') }}" class="btn bg-teal-400 btn-labeled">
      <b class="fa fa-pencil-square-o"></b> {{ trans('module-yourmodule::photogallery/form.btn_create') }}
    </a>
  @endpermission
@endsection

@section('breadcrumb-elements')
  @permission('module_yourmodule-create')
    <li>
      <a href="{{ route('module.module_yourmodule.photogallery.create') }}">
        <i class="fa fa-pencil-square-o"></i> {{ trans('module-yourmodule::photogallery/form.btn_create') }}
      </a>
    </li>
  @endpermission
@endsection
```

```php 
<?php

namespace Modules\YourModel\Components\Forms;

use App\Components\Forms\AbstractForm;
use Modules\OrganisationUnit\Models\Photogallery\Photogallery;

class PhotogalleryForm extends AbstractForm
{
    /**
     * View name.
     *
     * @var string
     */
    protected $view = 'module-yourmodule::admin.photogallery.form';

    /**
     * Photogallery.
     *
     * @var \Modules\YourModule\Models\Photogallery\Photogallery
     */
    protected $photogallery;

    /**
     * Mix Manifest Directory
     *
     * @var mixed
     */
    protected $mixManifestDirectory = 'modules/YourModule';

    /**
     * Photogallery form.
     *
     * @param Photogallery $photogallery
     * @throws \Exception
     */
    public function __construct(Photogallery $photogallery)
    {
        parent::__construct();
        $this->photogallery = $photogallery;

        $this->addScript(url('plugin/js/bootstrap-maxlength.js'));
        $this->addScript(mix('photogalleries.form.js', $this->mixManifestDirectory));
    }

    /**
     * Get view data.
     *
     * @return array
     */
    public function getViewData(): array
    {
        return [
            'photogallery' => $this->photogallery,
            'formDataJson' => $this->photogallery->getFormAttributesJson([
                'title', 'url', 'text', 'open_graph',
                'seo_title', 'seo_description', 'seo_index', 'seo_follow', 'seo_sitemap'
            ]),
            'submitUrl' => $this->photogallery->exists ?
                route('module.yourmodule.photogallery.update', ['photogallery' => $this->photogallery]) :
                route('module.yourmodule.photogallery.store'),
            'photos' => $this->photogallery->photos()->with('image')->get(),
            'cancelUrl' => route('module.your_module.photogallery.edit', [$this->photogallery]),
        ];
    }
}
```

Here is same situation like in `PhotogalleryTable`. Nothing special is in `PhotogalleryForm` what it's necessary to explain
more. With this form, we need to create `photogalleries.form.js` and `admin/photogallery/form.blade.php` view:

```js
import Form from '../../../../../../resources/assets/js/vendor/Form';
import SeoInputs from '../../../../../../resources/assets/js/vue-components/form/seo-inputs';
import OpenGraphInputs from '../../../../../../resources/assets/js/vue-components/form/open-graph-inputs';
import Photogallery from '../../../../../../resources/assets/js/vue-components/photogallery';

Vue.component('yourmodule-photogallery-form', {
    data() {
        return {
            form: new Form({
                ...this.photogallery
            }).addDataCollector(this.getFormData)
        };
    },

    props: {
        photogallery: {
            type: Object,
            required: true
        }
    },

    components: {
        'photogallery': Photogallery,
        'seo-inputs': SeoInputs,
        'open-graph-inputs': OpenGraphInputs,
    },

    methods: {
        /**
         * Fired when title is changed.
         * @param {Event} $event
         */
        onTitleChanged($event) {
            this.form.title = $event.target.value;

            if (this.form.url === null || !this.form.url.length) {
                this.form.url = this.form.title;
            }
        },

        /**
         * Fired when url is changed.
         * @param {Event} $event
         */
        onUrlChanged($event) {
            this.form.url = $event.target.value;
        },

        getFormData() {
            return {
                photogallery: this.$refs.photogallery.getFormData()
            };
        }
    },

    watch: {
        'form.url'(newUrl) {
            this.form.url = Converter.removeDiacritics(newUrl);
        }
    }

});
```

In this javascript file, here is one new Vue.js component for you - **Photogallery**, which builds a wonderful photo
uploader. Another things were used in the previous parts of this complete guide. If you want to get more information about them, go back
and read them above.

Now let's explain the source code for `admin/photogallery/form.blade.php`: 

```html
<?php /** @var \Modules\YourModule\Models\Photogallery\Photogallery $photogallery */ ?>
@extends('admin.layouts.master')

@section('content')
    <yourmodule-photogallery-form inline-template
                           :item="{{ $formValuesJson }}"
                           v-cloak>
        <div class="box-body">
            <v-form :form="form"
                    method="{{ $photogallery->exists ? 'PATCH' : 'POST' }}"
                    id="yourmodule-photogallery-form"
                    action="{{ $submitUrl }}">
                @include('module-yourmodule::admin.photogallery.form.layout')

                <div class="form-group mt15">
                    {!! Form::button($photogallery->exists ? trans('module-yourmodule::photogallery/form.btn_update') : trans('module-yourmodule::photogallery/form.btn_create'), [
                        'class' => 'btn bg-teal-400',
                        'type' => 'submit'
                    ]) !!}

                    <a href="{{ URL::previous() }}" class="btn btn-default">
                        {{ trans('module-yourmodule::photogallery/form.btn_cancel') }}
                    </a>
                </div>
            </v-form>
        </div>
    </yourmodule-photogallery-form>
@endsection

@push('style')
    @include('admin.vendor.form._styles')
@endpush

@push('script')
    @include('admin.vendor.form._scripts')
@endpush
```

`admin/photogallery/form/layout.blade.php`
```html
<?php /** @var \Modules\YourModule\Models\Photogallery\Photogallery $photogallery */ ?>

@include('admin.vendor.form.panel_errors')

<v-tabs class="nav-tabs-custom" no-fade>
    {{-- General --}}
    <v-tab title="{{ trans('module-yourmodule::photogallery/form.tabs.details') }}"
           href="#general"
           active>
        @include('module-yourmodule::admin.photogallery.form._tab_details')
    </v-tab>

    {{-- Photogallery --}}
    <v-tab title="{{ trans('module-yourmodule::photogallery/form.tabs.photogallery') }}"
           href="#photogallery"
    >
      @include('module-yourmodule::admin.photogallery.form._tab_photogallery')
    </v-tab>

    {{-- SEO --}}
    <v-tab title="{{ trans('module-yourmodule::photogallery/form.tabs.seo') }}"
           href="#seo"
    >
      @include('module-yourmodule::admin.photogallery.form._tab_seo')
    </v-tab>

    {{-- OpenGraph --}}
    <v-tab title="{{ trans('module-yourmodule::photogallery/form.tabs.og') }}"
           href="#open-graph"
    >
      @include('module-yourmodule::admin.photogallery.form._tab_og_tags')
    </v-tab>
</v-tabs>
```

`admin/photogallery/form/layout.blade.php`
```html
<?php /** @var \Modules\YourModule\Models\Photogallery\Photogallery $photogallery */ ?>

@include('admin.vendor.form.panel_errors')

<v-tabs class="nav-tabs-custom" no-fade>
    {{-- General --}}
    <v-tab title="{{ trans('module-yourmodule::photogallery/form.tabs.details') }}"
           href="#general"
           active>
        @include('module-yourmodule::admin.photogallery.form._tab_details')
    </v-tab>

    {{-- Photogallery --}}
    <v-tab title="{{ trans('module-yourmodule::photogallery/form.tabs.photogallery') }}"
           href="#photogallery"
    >
      @include('module-yourmodule::admin.photogallery.form._tab_photogallery')
    </v-tab>

    {{-- SEO --}}
    <v-tab title="{{ trans('module-yourmodule::photogallery/form.tabs.seo') }}"
           href="#seo"
    >
      @include('module-yourmodule::admin.photogallery.form._tab_seo')
    </v-tab>

    {{-- OpenGraph --}}
    <v-tab title="{{ trans('module-yourmodule::photogallery/form.tabs.og') }}"
           href="#open-graph"
    >
      @include('module-yourmodule::admin.photogallery.form._tab_og_tags')
    </v-tab>
</v-tabs>
```

I guess you know already how you can implement `admin/photogallery/form/_tab_details.blade.php`, `admin/photogallery/form/_tab_seo.blade.php` and
`admin/photogallery/form/_tab_og_tags.blade.php` views. But what about `admin/photogallery/form/_tab_photogallery.blade.php` source code? Here we go.

```html
<photogallery ref="photogallery"
              :photos="{{ isset($photos) ? $photos->toJson() : '[]' }}"
              :trans="{{ json_encode(trans('admin/general.photogallery')) }}"
></photogallery>
```

In the `_tab_photogallery.blade.php` view above, there is just using **Photogallery component**, which we imported by `photogalleries.form.js`. For this time, it's
done with an implementation of the photogallery form.

Let's continue for the next step and look the source code of `PhotogalleryRequest`:

```php
<?php

namespace Modules\YourEntity\Http\Requests\Admin;

use App\Contracts\PhotogalleryInterface;
use App\Http\Requests\AbstractFormRequest;
use Modules\OrganisationUnit\Models\Photogallery\Photogallery;
use App\Traits\Requests\ValidatesAndReceivesPhotogalleryTrait;
use App\Traits\Requests\ValidatesOpenGraphTrait;
use App\Traits\Requests\ValidatesSeoTrait;

class PhotogalleryRequest extends AbstractFormRequest
{
    use ValidatesAndReceivesPhotogalleryTrait,
        ValidatesSeoTrait,
        ValidatesOpenGraphTrait;

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        $rules = [
            'title' => 'required|max:191',
            'url' => 'required|max:191'
        ];

        return $this->mergeRules($rules, $this->getPhotogalleryRules(), $this->getSeoRules(), $this->getOpenGraphRules());
    }

    /**
     * Get the validation messages.
     *
     * @return array
     */
    public function messages()
    {
        return $this->mergeMessages(
            'module-yourmodule::photogallery/form.messages', $this->getSeoMessages(), $this->getOpenGraphMessages()
        );
    }

    /**
     * Return input values
     *
     * @return array
     */
    public function getValues(): array
    {
        $input = $this->all([
            'title', 'url', 'text', 'open_graph',
            'seo_title', 'seo_description'
        ]);

        // SEO index
        $input['seo_index'] = $this->input('seo_index', false);

        // SEO follow
        $input['seo_follow'] = $this->input('seo_follow', false);

        // SEO sitemap
        $input['seo_sitemap'] = $this->input('seo_sitemap', false);

        return $input;
    }

    /**
     * Get instance of model using photogallery.
     *
     * @return \App\Contracts\PhotogalleryInterface
     */
    protected function getModelWithPhotogallery(): PhotogalleryInterface
    {
        /** @var Photogallery $photogallery */
        $photogallery = $this->route('photogallery');
        return $photogallery ?? new Photogallery;
    }
}
```

Here is one new trait - `App\Traits\Requests\ValidatesAndReceivesPhotogalleryTrait`. This trait adds useful methods for your
form class - mainly validation rules and collection of input photos (it's easier for storing them to a database then).

After that, let's make our `PhotogalleriesController`:

```php
<?php

namespace Modules\YourModule\Http\Controllers\Admin;

use App\Http\Controllers\Admin\AdminController;
use Illuminate\Database\Eloquent\Model;
use Modules\YourModule\Http\Requests\Admin\PhotogalleryRequest;
use Modules\YourModule\Models\Photogallery\Photogallery;
use Modules\YourModule\Components\Forms\PhotogalleryForm;

class PhotogalleriesController extends AdminController
{
    /**
     * Active menu item nickname.
     *
     * @var string
     */
    protected $activeMenuItem;

    /**
     * PhotogalleriesController constructor.
     */
    public function __construct()
    {
        parent::__construct();

        $this->middleware('permission:module_yourmodule-create')
            ->only(['create', 'store']);

        $this->middleware('permission:module_yourmodule-edit')
            ->only(['edit', 'update']);

        $this->middleware('permission:module_yourmodule-edit')
            ->only(['updatePhoto', 'photoList', 'deletePhoto']);

        $this->middleware('permission:module_yourmodule-delete')
            ->only('delete');
    }

    /**
     * GET: Index
     *
     * @param \Illuminate\Http\Request $request
     * @return \Illuminate\View\View
     */
    public function index(Request $request)
    {
        $this->setTitleDescription(
            trans('module-yourmodule::photogallery/admin.pages.index.title'),
            trans('module-yourmodule::photogallery/admin.pages.index.description'));

        $table = new PhotogalleryTable($this->getUser());
        return $table->toResponse($request, 'module-yourmodule::admin.photogallery.index');
    }

    /**
     * GET: Create
     *
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function create()
    {
        $this->setTitleDescription(
            trans('module-yourmodule::photogallery/admin.pages.create.title'),
            trans('module-yourmodule::photogallery/admin.pages.create.description')
        );

        $photogallery = new Photogallery();
        $photogallery->forceFill([
            'seo_index' => true,
            'seo_follow' => true,
            'seo_sitemap' => true,
        ]);

        $form = new PhotogalleryForm($photogallery);
        return $form->getView();
    }

    /**
     * POST: Store
     *
     * @param PhotogalleryRequest $request
     * @return \Illuminate\Http\RedirectResponse
     */
    public function store(PhotogalleryRequest $request)
    {
        // Create photogallery
        $photogallery = Photogallery::query()->create($request->getValues());
        $photogallery->save();

        $photogallery->savePhotogallery($request->getPhotogallery());

        flash(trans('module-yourmodule::photogallery/general.notifications.created'), 'success');
        return $this->redirect(route('module.yourmodule.photogallery.index'));
    }

    /**
     * GET: Edit
     *
     * @param Photogallery $photogallery
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function edit(Photogallery $photogallery)
    {
        $this->setTitleDescription(
            trans('module-yourmodule::photogallery/admin.pages.edit.title'),
            trans('module-yourmodule::photogallery/admin.pages.edit.description'));

        $form = new PhotogalleryForm($photogallery);
        return $form->getView();
    }

    /**
     * POST: Update
     *
     * @param PhotogalleryRequest $request
     * @param Photogallery $photogallery
     * @return \Illuminate\Http\RedirectResponse
     */
    public function update(PhotogalleryRequest $request, Photogallery $photogallery)
    {
        // Save values
        $photogallery->update($request->getValues());
        $photogallery->savePhotogallery($request->getPhotogallery());

        flash(trans('module-yourmodule::photogallery/general.notifications.updated'), 'success');
        return $this->redirect(route('module.yourmodule.photogallery.edit', [$photogallery->id]));
    }

    /**
     * DELETE: Delete
     *
     * @param Photogallery $photogallery
     * @return \Illuminate\Http\JsonResponse|\Illuminate\Http\RedirectResponse
     * @throws \Exception
     */
    public function delete(Photogallery $photogallery)
    {
        $photogallery->delete();

        flash(trans('module-yourmodule::photogallery/general.notifications.deleted'), 'success');
        return $this->refresh();
    }

}
```

## Third Party Libraries

When you need to install third-party libraries using `composer.json`, then it's necessary to insert the source code below inside
`start.php` file:

```php
<?php

// Register The Auto Loader
require __DIR__.'/vendor/autoload.php';
```