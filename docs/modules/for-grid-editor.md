---
id: for-grid-editor
title: For Grid Editor
---

## Introduction

In [Modules / Complete Guide](complete-guide.md), you can get an information about implementing modules for a new entity.
If you want to extend directly Grid Editor about new possible module, this is the right page where you can learn it.

A module designed for Grid Editor must meet several criteriums. The most important criteurim is mainly needed to make its entity.

## First Steps

For the first step, it is necessary to make a new module's directory. It's simple more than for a module with entity, because 
all SIMPLO CMS default modules are designed for Grid Editor only. It means that is all one which one default module you will
choose for copying and starting position for your new Grid Editor designed module.

Let's make a copy from some SIMPLO CMS default module and removed directories / files for the same directory structure like
on the scheme below:

```text
.
├── Assets
|   └── js
|       └── configuration.js
├── Config
|   └── config.php
├── Database
|   └── Migrations
├── Dist
├── Http
|   ├── Controllers
|   |   └── ModuleController.php
|   ├── Requests
|   |   └── ModuleRequest.php
|   └── routes.php
├── Models
|   ├── Configuration.php
|   └── GridEditorForm.php
├── Providers
|   └── ServiceProvider.php
├── Resources
|   ├── lang
|   |   └── en
|   |       └── admin.php
|   └── views
|       ├── configuration
|       |   └── form.blade.php
|       ├── missing_view.blade.php
|       └── module_preview.blade.php
├── .gitignore
├── composer.json
├── module.json
├── package.json
├── README.md
├── start.php
└── webpack.mix.js
```

For this first step, we need to make a few changes inside the files as well. So now, please open `Config/config.php` file
and change it similar like that:

```php 
<?php

return [
    'name' => 'YourModuleList',
    'icon' => 'comments',

    'for-grideditor' => true,

    'grideditor_title' => 'module-yourmodulelist::admin.grideditor_title'
];
```

Be attention that you need to set `for-grideditor` value to `true`. In this guide, we are making only Grid Editor module.
When you will set everything in `config.php` file, move to `ServiceProvider` and rename everything from copied `ArticlesList` module.
We do not need to make anything special there and really, it's necessary to rename only namespaces.

> Be careful that you need to keep `module-` prefix before a namespace. SIMPLO CMS system can recognize your module properly.

When you finish works above, open `composer.json` file and again, rename all namespaces there. Pay attention about correct `psr-4`
autoload setting.

After changes in `composer.json` file, open a next json file `module.json` and rename everything there as well. In this file, there is 
very useful a key `providers`, where you can define more providers of your module. Then it's not necessary to boot or register new services
inside the main `ServiceProvider` provider.

In root's directory here is located `package.json` file. Probably **for Grid Editor only module**, you will not want to change anything
there because when you made a copy from `ArticlesList` SIMPLO CMS default module, everything is ready for developing right.

As well pay attention for change `README.md` file. Please help to another developers and make useful description of your new module.

> **IMPORTANT!** It's important to have a module just designed for Grid Editor only or use for another purpose. Do not mix
> an entity module with Grid Editor module only. For entity module implementation, please visit [Modules / Complete Guide](complete-guide.md).

## How To Make - Database, Models, Controllers, Compiling Assets, Events

Before continue in this guide, you must learn, how to implement a database migration, a model, a controller, an event and compiling assets. 
For this study, you can visit these pages:

- Database - [link](modules/database.md)
- Models - [link](modules/models.md)
- Routing - [link](modules/routing.md)
- Controllers - [link](modules/controllers.md)
- Compiling Assets - [link](modules/compiling-assets.md)
- Events - [link](modules/events.md)

It's a good idea to read all pages above before reading the next rows.

## Implementation

Now it's important to decide for which purpose you will use your new Grid Editor module. When you will check default SIMPLO CMS modules 
for Grid Editor, you can notice that there is one common database migration for all these modules - `configurations`. It's a table
where will be stored all configuration for module's entities, about which we mentioned in the introduction of this guide. 

Second thing for decision, how will look our configuration table, is a purpose for which we will implement this Grid Editor module.
We can try to make in this example a module which will render a view with some model items. It's possible to understand that we 
make a module which will offer to us (administrators) an option for printing items from our model items in some Grid Editor content 
on public website then. Let's start to develop it.

For the first step of course, we need to create a database migration:

```php 
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateModuleYourEntityListConfigurations extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('module_yourentity_list_configurations', function (Blueprint $table) {
            $table->increments('id');

            $table->string('view');
            $table->unsignedInteger('limit')->nullable();

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
        Schema::dropIfExists('module_yourentity_list_configurations');
    }
}
```

For our Grid Editor module, we will be able to select a render view and a limit of the printed items as well. Then, it's 
necessary to define some route paths in `Http/routes.php`:

```php
<?php

Route::group([
    'middleware' => 'web',
    'prefix' => 'admin/module/your-entity-list',
    'namespace' => 'Modules\YourModuleList\Http\Controllers'
], function () {

    Route::get('create', [
        'as' => 'module.yourentitylist.create',
        'uses' => 'ModuleController@create',
    ]);

});
```

> For Grid Editor module, it's not necessary to define more routes. Enough is just only define the `create` route.

Now we need to make models for our Grid Editor module. First, we will create `Configuration` module

```php 
<?php

namespace Modules\YourModuleList\Models;

use App\Models\Interfaces\ModuleConfigurationInterface;
use App\Models\Module\Module;
use App\Structures\Enums\SingletonEnum;
use App\Traits\AdvancedEloquentTrait;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Support\Collection;
use Illuminate\Support\Facades\View;
use Modules\YourModuke\Models\YourEntity;

/**
 * Class Configuration
 * @package Modules\YourModuleList\Models
 * @author SIMPLO, s.r.o.
 * @copyright SIMPLO, s.r.o.
 *
 * @property string view
 * @property int limit
 */
class Configuration extends Model implements ModuleConfigurationInterface
{
    use AdvancedEloquentTrait;

    /**
     * @var string Table name of the model
     */
    protected $table = 'module_yourentity_list_configurations';

    /**
     * The attributes that should be cast to native types.
     *
     * @var array
     */
    protected $casts = [
        'limit' => 'int',
    ];

    /**
     * Mass assignable attributes
     *
     * @var array
     */
    protected $fillable = ['view', 'limit'];

    /**
     * Get default configuration
     *
     * @return Configuration
     */
    public static function getDefault()
    {
        return new self([
            'limit' => 0
        ]);
    }

    /**
     * @return \Illuminate\Support\Collection|YourEntity[]
     */
    private function getItems(): Collection
    {
        $query = YourEntity::query();

        if ($this->limit) {
            $query->limit($this->limit);
        }

        $items = $query->get();

        return $items;
    }

    /**
     * Render module
     *
     * @param array $renderAttributes
     * @return string
     * @throws \Throwable
     */
    public function render(array $renderAttributes = []): string
    {
        if (!View::exists($this->view)) {
            return view('module-yourentitylist::missing_view', ['name' => $this->view])->render();
        }

        $configuration = $this;
        $items = $this->getItems();
        return view($this->view, compact('configuration', 'items'))->render();
    }

    /**
     * Fill model with input values with mutators inside.
     *
     * @param array $inputs
     * @return $this
     */
    public function inputFill(array $inputs)
    {
        return $this->fill($inputs);
    }

    /**
     * @return \App\Models\Module\Module
     */
    public function getModule(): Module
    {
        return SingletonEnum::modules()->find('YourModuleList');
    }
}
```

The `getItems` method serves for loading items of our entity. This is entity from another module, which is designed for
making entity items only. How we mentioned in the introduction of this tutorial, we must make a module only for Grid Editor
or for increase our application about new entity (entities). For our example Grid Editor module, we provide a way how
administrators can render a list of these items from entity on a public web. It's reason why we use in `getItems` method
a model from another module (entity module).

> If you want to know how you can **make an entity module**, please visit [Modules / Complete Guide](complete-guide.md).

For the next step, we need to make `GridEditorForm` model as well:

```php 
<?php

namespace Modules\YourModuleList\Models;

use App\Components\Forms\AbstractForm;
use App\Helpers\ViewHelper;
use App\Models\Web\Language;

class GridEditorForm extends AbstractForm
{
    /**
     * View name.
     *
     * @var string
     */
    protected $view = 'module-yourmodulelist::configuration.form';

    /**
     * Configuration.
     *
     * @var Configuration
     */
    protected $configuration;

    /**
     * @var \App\Models\Web\Language
     */
    private $language;

    /**
     * Configuration form for grid editor.
     *
     * @param Configuration $configuration
     * @param \App\Models\Web\Language $language
     * @throws \Exception
     */
    public function __construct(Configuration $configuration, Language $language)
    {
        parent::__construct();
        $this->configuration = $configuration;
        $this->language = $language;

        $this->addScript($configuration->getModule()->mix('configuration.js'));
    }

    /**
     * Get view data.
     *
     * @return array
     */
    public function getViewData(): array
    {
        $views = ViewHelper::getDemarcatedViews('modules.your_entity_list');

        return \array_merge([
            'configuration' => $this->configuration
        ], \compact('views'));
    }
}
```

The `getViewData` method returns all available views, which administrators will be able to select in Grid Editor content then.
All these views we can define and then store in `themes/{our_theme}/resources/views/modules/your_entity_list` directory. For this
view rendering, the system will use the `render` method in our `Configuration` model. When you will check this method again in the previous
source code, you can notice that we are giving to the selected view two variables - `$configuration` and `$items`. With these variables, you will be able to
work inside all defined views from `themes/{our_theme}/resources/views/modules/your_entity_list` directory. Probably in our example list, here will be
foreach cycle.

After the definition of our models, it's convenient to make a view for showing form inside Grid Editor, when an administrator will choose this
module for his Grid Editor content. Make the `configuration/form.blade.php` file and implement a source code similar like this below:

```html
{{ Form::model($configuration, ['id' => 'model-yourmodulelist-configuration-form']) }}

<div class="alert alert-warning" v-if="!hasViews">
    {{ trans('module-yourmodulelist::admin.grid_editor_form.no_views') }}
</div>

<template v-else>
    {{-- View --}}
    <v-form-group :required="true" :error="form.getError('view')">
        <label for="mv-yourmodulelist-input-view">
            {{ trans('module-yourmodulelist::admin.grid_editor_form.labels.view') }}
        </label>
        {{ Form::select('view', $views, null, [
            'class' => 'form-control',
            'id' => 'mv-yourmodulelist-input-view',
            'v-model' => 'form.view'
        ]) }}
    </v-form-group>

    {{-- Limit --}}
    <v-form-group :error="form.getError('limit')">
        <label for="mv-yourmodulelist-input-limit">
            {{ trans('module-yourmodulelist::admin.grid_editor_form.labels.limit') }}
        </label>
        {{ Form::number('limit', null, [
            'class' => 'form-control',
            'id' => 'mv-yourmodulelist-input-limit',
            'v-model' => 'form.limit',
            'min' => 0
        ]) }}
    </v-form-group>
</template>

{{ Form::close() }}

<script>
    window.YourModuleListModuleOptions = function () {
        return {!! json_encode([
            'model' => $configuration->only(['view', 'limit']),
            'views' => $views,
            'trans' => trans('module-yourmodulelist::admin.grid_editor_form')
        ]) !!};
    };
</script>

@include('admin.vendor.form._scripts')
```

The `$configuration` variable, which is inserted to the `model` method, will be transmitted to this view using `ModuleController` controller.
This `ModuleController` we will implement later in this guide, but it's good you know about it now. Probably how you guess well, this
`$configuration` variable contains an instance of `Configuration` model.

Elements inside this form are understandable and there is nothing special. Just only input elements which we decided to have
for our Grid Editor only module.

> When you **will not define any views** inside the `themes/{our_theme}/resources/views/modules/your_entity_list` directory, administrators will see
> only a message about no views after what they chose your module for their Grid Editor content. For sure, you are able to change this behavior
> with `v-if` Vue.js condition and `hasViews` variable, but it's a convenient way to keep this behavior because of more comfortable
> work with Grid Editor modules.

Good for description of this view file is a javascript code inside the `<script>` element. The `window.YourModuleListModuleOptions` 
function is using in the `Assets/js/configuration.js` file, whose source code is shown below:

```js
import Form from "../../../../../../resources/assets/js/vendor/Form";

const options = window.YourModuleListModuleOptions();

new Vue({
    el: '#model-yourmodulelist-configuration-form',

    data: {
        localization: new Localization({...window.cms_trans, ...options.trans}),
        form: new Form(options.model),
        $form: null,
    },

    computed: {
        hasViews() {
            const views = options.views;
            return Array.isArray(views) ? views.length : Object.keys(views).length;
        }
    },

    methods: {
        /**
         * Set loaded fields + initialize variables.
         * @param {Object[]} fields
         */
        setFields(fields) {
            this.fields = [];
        },

        /**
         * Fill form with data from GridEdtior.
         * @param {Object} data
         */
        fillForm(data) {
            this.form = new Form({...data});
        },

        /**
         * Give form data to GridEditor.
         * @param {Event} event
         * @param {Object} output - shared object
         */
        getFormData(event, output) {
            output._form = this.form.getData();
        },
    },

    mounted() {
        this.$form = $(this.$el);
        // Form fill
        this.$form.trigger('admin:form-fill-ready', this.fillForm);
        this.$form.on('admin:form-submit-data', this.getFormData);
    },

    beforeDestroy() {
        this.$form.off('admin:form-submit-data', this.getFormData);
    },
});
```

How you can notice in this source code of the `Assets/js/configuration.js` javascript file, the `window.YourModuleListModuleOptions` function
is used for filling in the configuration form, for calculating available views and for localization.

In views, we need to define two another views yet. First, this is a view for printing only missing view message - `missing_view.blade.php`:

```html
<div style="color: #a94442;background-color: #f2dede;padding: 15px;margin: 5px;border: 1px solid #ebccd1;">
    {{ trans('module-faqcategorylist::admin.view_not_exist', ['name' => $name]) }}
</div>
```

This view will be rendered when a view, what an administrator selected, was removed. You can check the source code of
the rendering inside the `render` method of `Configuration` module.

A second another view is `module_preview.blade.php`, which prints a module preview inside Grid Editor when an administrator
selected and set your Grid Editor module:

```html
<?php
/** @var \Modules\YourModuleList\Models\Configuration $configuration */
$viewName = \App\Helpers\ViewHelper::getViewName('modules.your_module_list', $configuration->view);
$limit = trans_choice('module-yourmodulelist::admin.preview.limit', $configuration->limit, [
    'count' => $configuration->limit
]);
?>
<strong>{{ trans('module-yourmodulelist::admin.preview.labels.view') }}:</strong>
@if($viewName)
    {{ $viewName }}
@else
    <span class="text-danger">{{ trans('module-yourmodulelist::admin.preview.not_found') }}</span>
@endif
<br>
<strong>{{ trans('module-yourmodulelist::admin.preview.labels.limit') }}:</strong> {{ $limit }}<br>
```

When you see the source code above, it's all clear.

> For all available Grid Editor module view names, you are able to use a localization which can be defined inside your theme.
> TODO MORE!!!

Before an implementing the `ModuleController` controller, we need to implement a last class - `ModuleRequest`.

```php
<?php

namespace Modules\YourModuleList\Http\Requests;

use App\Helpers\ViewHelper;
use App\Http\Requests\AbstractFormRequest;
use Illuminate\Validation\Rule;

class ModuleRequest extends AbstractFormRequest
{
    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'view' => ['required', Rule::in(ViewHelper::getDemarcatedViewsKeys('modules.your_module_list'))],
            'limit' => [
                'nullable',
                'int',
                'min:0'
            ]
        ];
    }

    /**
     * @return string[]
     */
    public function messages()
    {
        return trans('module-yourmodulelist::admin.grid_editor_form.messages');
    }
}
```

In the `ModuleRequest` is just one thing what deserves a description - rules of `view`. The first rule - `required` - is
making the view input mandatory. For the second rule, we want to have in the view input only demarcated views, which we defined
in the `themes/{our_theme}/resources/views/modules/your_entity_list` directory. Because of this reason, we need to check a list of this 
possible views for a selecting. For this purpose, we must use the `Illuminate\Validation\Rule` class and the method `in`, where we transmit
all these views. SIMPLO CMS offers the `getDemarcatedViewsKeys` static method of the `App\Helpers\ViewHelper` helper for getting keys of all 
demarcated views.

For now, we have prepared database migrations, models, route paths, views, assets and request. It's an ideal starting position for implementing 
`ModuleController` controller, which handles all processes. For our example, the `ModuleController` can have the following source code:

```php
<?php

namespace Modules\YourModuleList\Http\Controllers;

use App\Http\Controllers\Admin\AdminController;
use Modules\YourModuleList\Http\Requests\ModuleRequest;
use Modules\YourModuleList\Models\Configuration;
use Modules\YourModuleList\Models\GridEditorForm;

class ModuleController extends AdminController
{
    /**
     * Show the form for creating a new resource.
     * @return \Modules\YourModuleList\Models\GridEditorForm
     * @throws \Exception
     */
    public function create()
    {
        return new GridEditorForm(Configuration::getDefault(), $this->getLanguage());
    }

    /**
     * Validate configuration and return preview.
     *
     * @param \Modules\YourModuleList\Http\Requests\ModuleRequest $request
     * @return \Illuminate\Http\JsonResponse
     * @throws \Throwable
     */
    public function validateAndPreview(ModuleRequest $request)
    {
        $configuration = new Configuration();
        $configuration->inputFill($request->only(['view', 'limit']));

        return response()->json([
            'content' => view('module-yourmodulelist::module_preview', compact('configuration'))->render()
        ]);
    }

    /**
     * Show the form for editing specified resource.
     *
     * @param \Modules\YourModuleList\Models\Configuration $configuration
     * @return \Modules\YourModuleList\Models\GridEditorForm
     * @throws \Exception
     */
    public function edit(Configuration $configuration)
    {
        return new GridEditorForm($configuration, $this->getLanguage());
    }
}
```

In the `create` method, we need to return a form with a new instance of `Configuration` model. The `validateAndPreview` method
is calling everytime when an administrator will send the form of our example module in Grid Editor. This method must return
a module preview view because then this result is rendered inside Grid Editor after finishing validation process by the `ModuleRequest`
request. The `edit` method is similar like `create` but already load an existed item instead of create a new one.

If **you did everything according to this guide**, you will be able to install and use your new module inside Grid Editor. Congratulations!