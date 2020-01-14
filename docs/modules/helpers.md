---
id: helpers
title: Helpers
---

## Introduction

SIMPLO CMS offers including [Core Helpers](../core/helpers.md) also some helpers for [Modules](../modules/general.md).
And just about them you can find out more on the next rows.

## Available Helpers

**`module_path($name)`**

The `module_path` function returns a direct path into the module, which will be defined as the `$name` parameter. It's only this parameter,
which the `module_path` function requires.

Return example: 
```php
// /Applications/MAMP/htdocs/simplo-cms/modules/Blog
echo module_path('Blog');
```

**`Module::all()`**

The `all` method returns all available modules.

Return example: 
```php
use Nwidart\Modules\Facades\Module;

// {"ArticlesList":{},"Blog":{},"Image":{},"Link":{},"Photogallery":{},"Text":{},"View":{}}
echo json_encode(Module::all());
```

> TODO - update example