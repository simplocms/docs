---
id: helpers
title: Helpers
---

## Introduction

SIMPLO CMS offers including [Core Helpers](../core/helpers.md) also some helpers for [Modules](../modules/general.md).
And just about them you can find out more on the next rows.

## Available Helpers

**`module_path($name): string`**

The `module_path` function returns a direct path into the module, which will be defined as the `$name` parameter. It's only this parameter,
which the `module_path` function requires. You can also use `getModulePath` static method on the `Nwidart\Modules\Facades\Module` facade.

Return example: 
```php
// /Applications/MAMP/htdocs/simplo-cms/modules/Blog
echo module_path('Blog');
```

**`Module::all(): array`**

The `all` method returns all available modules.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

// {"ArticlesList":{},"Blog":{},"Image":{},"Link":{},"Photogallery":{},"Text":{},"View":{}}
echo json_encode(Module::all());
```

**`Module::getOrdered(): array`**

The `getOrdered` method returns ordered modules by the `order` key in `module.json`.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

// {"ArticlesList":{},"Blog":{},"Image":{},"Link":{},"Photogallery":{},"Text":{},"View":{}}
echo json_encode(Module::getOrdered());
```

**`Module::find($name)`**

The `find` method returns the specific module. If the specific module exists, then it's returned `App\Models\Module\Module` instance, otherwise null
value is returned. This method accepts only one `$name` parameter with the specific module's name.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

dd(Module::find('Blog'));
```
```text
Module {#2107 ▼
  #app: Application {#8 ▶}
  #name: "Blog"
  #path: "/Applications/MAMP/htdocs/simplo-cms/modules/Blog"
  #moduleJson: []
  #defer: false
}
```

**`Module::findOrFail($name)`**

The `findOrFail` method returns the specific module. If the specific module exists, then it's returned `App\Models\Module\Module` instance, otherwise
the application throws `Nwidart\Modules\Exeptions\ModuleNotFoundException` exception. This method accepts only one `$name` parameter
with the specific module's name.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

dd(Module::findOrFail('Blog'));
```
```text
Module {#2107 ▼
  #app: Application {#8 ▶}
  #name: "Blog"
  #path: "/Applications/MAMP/htdocs/simplo-cms/modules/Blog"
  #moduleJson: []
  #defer: false
}
```

**`Module::toCollection(): Collection`**

The `toCollection` method returns all available modules as `Nwidart\Modules\Collection` instance, which extends base Laravel `Collection`.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

dd(Module::toCollection());
```
```text
Collection {#2099 ▼
  #items: array:7 [▼
    "ArticlesList" => Module {#2104 ▶}
    "Blog" => Module {#2108 ▶}
    "Image" => Module {#2112 ▶}
    "Link" => Module {#2116 ▶}
    "Photogallery" => Module {#2120 ▶}
    "Text" => Module {#2124 ▶}
    "View" => Module {#2128 ▶}
  ]
}
```

**`Module::getByStatus($status): array`**

The `getByStatus` method returns all modules by the given `$status` mandatory parameter. The status `1` returns active modules and the status `0` returns inactive modules.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

dd(Module::getByStatus(1));
```
```text
array:7 [▼
  "ArticlesList" => Module {#2103 ▶}
  "Blog" => Module {#2107 ▶}
  "Image" => Module {#2111 ▶}
  "Link" => Module {#2115 ▶}
  "Photogallery" => Module {#2119 ▶}
  "Text" => Module {#2123 ▶}
  "View" => Module {#2127 ▶}
]
```

**`Module::allEnabled(): array`**

The `allEnabled` method returns enabled modules. It means, that the `active` key of the modules has value `1`. It's also possible to use the `getByStatus`
method for the same result.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

dd(Module::allEnabled());
```
```text
array:7 [▼
  "ArticlesList" => Module {#2103 ▶}
  "Blog" => Module {#2107 ▶}
  "Image" => Module {#2111 ▶}
  "Link" => Module {#2115 ▶}
  "Photogallery" => Module {#2119 ▶}
  "Text" => Module {#2123 ▶}
  "View" => Module {#2127 ▶}
]
```

**`Module::allDisabled(): array`**

The `allDisabled` method returns disabled modules. It means, that the `active` key of the modules has value `0`. It's also possible to use the `getByStatus`
method for the same result.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

dd(Module::allDisabled());
```
```text
[]
```

**`Module::config($key[, $default = null])`**

The `config` method returns a config value from this module package. The `$key` parameter is mandatory and requires
config key. The method also accepts one optional parameter:
- `$default` - it's value, which will be used in case, when the given package's config key doesn't exist

Return example: 
```php
use Nwidart\Modules\Facades\Module;

// Simplo.cz
echo Module::config('composer.author.name');
```

**`Module::assetPath($name): string`**

The `assetPath` method returns the asset path for the given module in a mandatory `$name` parameter.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

// /Applications/MAMP/htdocs/simplo-cms/public/modules/Blog
echo Module::assetPath('Blog');
```

**`Module::asset($asset): string`**

The `asset` method returns URL address to the given module's asset. The `$asset` parameter is mandatory and requires
the following format: `module-name:asset`. If the format is invalid, then the application throws `Nwidart\Modules\Exceptions\InvalidAssetPath` exception.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

// //localhost/modules/view/configuration.js
echo Module::asset('view:configuration.js');
```

**`Module::findRequirements($name)`**

The `findRequirements` method returns all required modules of the given module in `$name` mandatory parameter. If the given
module doesn't exist, then the application throws `Nwidart\Modules\Exceptions\ModuleNotFoundException` exception. The required
modules are defined in `module.json` file under the `requires` key.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

dd(Module::findRequirements('Blog'));
```
```text
[]
```

**`Module::macro($name, $macro)`**

The `macro` method belongs to the `Illuminate\Support\Traits\Macroable` trait and because of this module's package uses the trait, you can
use macros during working with modules. The working with macros in modules is the same like in Laravel. Just call the `macro` method
and pass two mandatory parameters, where the `$name` is the macro's name and the `$macro` is callable function, which will be called.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

Module::macro('customMacro', function () {
    echo 'Hello SIMPLO CMS!';
});

// Hello SIMPLO CMS!
Module::customMacro();
```

## Find More Helpers

If you want to get more helpers, which you can use during your development with modules, then try to visit official 
[nwidart/laravel-modules](https://nwidart.com/laravel-modules/v3/introduction) package.