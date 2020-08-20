---
id: universal-modules
title: Universal Modules
---

## Introduction

Universal modules are simpler modules, which can be defined easily inside [Configuration](../theme/configuration.md) file 
of [Theme](../theme/general.md). Unlike a classic module, you don't define database migrations, models, views and other things
for a universal module.

## Implementation

### Basic

How we mentioned in the introduction section, the universal modules are defined in a configuration `universal_modules.php` 
file of your theme. For making a universal module, you must use the `make` static method of `App\Services\UniversalModules\UniversalModule` 
class. For definition input fields, it's used [Dynamic Forms](../core/dynamic-forms.md).

You can use a universal module for [Grid Editor](../core/grid-editor.md) as well. For this implementation, it's needed to
specify a demarcated view for concrete items of the module. All these demarcated views are needed to locate inside the
`{your_theme}/view/universal_modules/{module_name}` directory.

In the source code below, you can see an example of the definition for a universal module `Banners`:

```php
<?php

return [

    \App\Services\UniversalModules\UniversalModule::make(
        'banners', 'theme::universal_modules.banners.name', 'film'
    )
        ->setFields([
            \App\Structures\FormFields\TextInput::make(
                'name', 'theme::universal_modules.banners.labels.name'
            )->required(),
            \App\Structures\FormFields\TextArea::make(
                'perex', 'theme::universal_modules.banners.labels.perex'
            )->required(),
            \App\Structures\FormFields\TextInput::make(
                'link', 'theme::universal_modules.banners.labels.link'
            )->required(),
            \App\Structures\FormFields\CheckboxSwitch::make(
                'is_video_link', 'theme::universal_modules.banners.labels.is_video_link'
            ),
            \App\Structures\FormFields\TextInput::make(
                'link_text', 'theme::universal_modules.banners.labels.link_text'
            )->required(),
            \App\Structures\FormFields\Image::make(
                'image', 'theme::universal_modules.banners.labels.image'
            )->required(),
        ]),
];
```

### Allow Ordering

If you need to have an ordering mechanism for your universal module, you don't have to implement it by yourself. It's just possible
to call the `allowOrdering()` method on the `App\Services\UniversalModules\UniversalModule` instance and everything is done. After this step,
you will be able to order items in your universal module with defining an ordering number.

### Localization

Sometimes you can wish that a universal module supports multilingual. It's possible to define this such kind of universal module
in SIMPLO CMS and with this multilingual mechanism, it can be worked with two ways:

1. A module will have for all available languages same items. A localization will be solved inside these items. It's a convenient way
in a case when we don't want that a user inserts items of the module for each language separately. For activate of this multilingual type,
you must call the `multilangual(\App\Services\UniversalModules\UniversalModule::MULTILANGUAL_TYPE_LOCALIZABLE)` method during a definition of this
universal module.
2. A module will have for all available languages different items. It's a good way when you wish to insert an item for each language separately and 
a new item will not be common for all available languages. for activate of this multilingual type, you must call the
`multilangual(\App\Services\UniversalModules\UniversalModule::MULTILANGUAL_TYPE_APART)` method during a definition of this universal module.

### Allow Toggling

If it's needed to have an option for activate / deactivate of module items, you can turn on this functionality using calling the
`allowToggling()` method. Then with the `scopeEnabled()` method, you can select just only activated items. A model instance of this activated item has
a writeable boolean `enabled` property.

### Custom URL

If it's need to have a module with a custom url address, it's possible to turn on this functionality by the `withUrl()` method.

### Rendering Items

After a definition of universal module, you will wish certainly to load and render items somewhere. For this purpose, you can make
a demarcated view and store it in the `{your_theme}/view/universal_modules/{module_name}` directory. This such stored demarcated view, there is available
a collection of the items under the `$items` property. Using this `$items` property, you are able to print everything what you need.

When you need to get some attribute of the item, use the `getAttributeOfContent(string $attribute, ?Language $language = null)` method on
`App\Models\UniversalModule\UniversalModuleItem` instance. This method returns this attribute from a unserialized content. 
The `App\Models\UniversalModule\UniversalModuleItem` provides more useful methods:

