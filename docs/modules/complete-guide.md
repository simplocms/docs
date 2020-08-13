---
id: complete-guide
title: Complete Guide
---

## Introduction

With modules, developers can extend an application and SIMPLO CMS about new entities or just extend only Grid Editor.
If you are interesting about these things and how you can make them, you are on the right page. Let's develop them together.

## Complete Guide - Make A New Entity

For this complete guide, we will describe a lot of features, what SIMPLO CMS offers for developers with modules.

### First Steps

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
Composer's autoload.

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

If you have **the same directory structure** for our example module **Module Name**, CONGRATULATIONS! Then you can continue
in the next steps about our module's developing with us!

> If you want, **you can use this starting position also for another modules**.

### How To Make - Database, Models, Controllers, Compiling Assets, Events

Before the following rows, you need to get more, how to define a database migration, a model, a controller, an event or how to compiling
assets. For these information, you can visit the following pages:
- Database - [link](modules/database.md)
- Models - [link](modules/models.md)
- Routing - [link](modules/routing.md)
- Controllers - [link](modules/controllers.md)
- Compiling Assets - [link](modules/compiling-assets.md)
- Events - [link](modules/events.md)

The best idea is when you will read all pages above. Then if you will know everything important, you can continue in this **Complete Guide**.

### Basic Implementation

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

### SEO, Open Graph

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

### Multilingual

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

### Model URL

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