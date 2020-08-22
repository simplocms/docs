---
id: default-modules
title: Default Modules
---

## Introduction

On this page, you can meet with default modules what SIMPLO CMS offers. It's not necessary to use or install these modules when you don't 
need them. 

## View

This module is determined for inserting a view directly to Grid Editor. It's convenient for creating more complex views with an option
for definition own properties using [Dynamic Forms](../core/dynamic-forms.md). This option provides for users a setting of these properties
using administration in Grid Editor. It means that a user does not need to contact a developer because of some changes.

> The view file **must be always** in UTF-8 format.

When you want to use your view with **View Module**, it's needed to register it. For registration of some view, you can move this
view file to the `{theme}/resources/views/modules/view` path. After this registration, a user will be able to select this view using
**View Module**. This attitude is called [Demarcated Views](../theme/views.md#demarcated-views). For this registration, it's a good way to
define a name of this demarcated view by a localization principle. More information about it, you can get on 
[Demarcated Views](../theme/views.md#demarcated-views).

When you need to define some form inputs for your demarcated view of **View Module**, you can use [Dynamic Forms](../core/dynamic-forms.md).
All user-configurable variables should follow the same naming convension - for example these names could start with `v_` prefix. This definition
of all user-configurable variables **must be on the first rows** of this demarcated view. If the view does not have any dynamic form inputs,
then it's not necessary to keep the source code header like on the example below.

```php
<?php

/* VARIABLES DESCRIPTION */
if (isset($GET_VARIABLES)) {
    return [
        \App\Structures\FormFields\NumberInput::make('v_some_number', 'Some number')
            ->min(15)->max(30)->step(5)->value(20),
        \App\Structures\FormFields\Select::make('v_select', 'Some select')
            ->required()
            ->options([
                1 => 'One',
                2 => 'Two',
                3 => 'Three'
            ])
            ->value(2),
        \App\Structures\FormFields\TextArea::make('v_textarea', 'Some TextArea')
            ->maxLength(150)->rows(5)
            ->value('default value'),
        \App\Structures\FormFields\TextInput::make('v_text', 'Some Text')
            ->maxLength(150)->placeholder("Placeholder text")
            ->value('default value'),
        \App\Structures\FormFields\CKEditor::make('v_ftext', 'Some Formatted text'),
        \App\Structures\FormFields\Input::make('email', 'v_email', 'Some email'),
    ];
}
?>

<strong>Proměnná "v_some_number":</strong> {{ $v_some_number }}
<strong>Proměnná "v_select":</strong> {{ $v_select }}
<strong>Proměnná "v_textarea":</strong> {{ $v_textarea }}
<strong>Proměnná "v_text":</strong> {{ $v_text }}
<strong>Proměnná "v_ftext":</strong> {{ $v_ftext }}
<strong>Proměnná "v_email":</strong> {{ $v_email }}
``` 

## ArticlesList

The `ArticlesList` module is designed for using in Grid Editor. It's a module which help to users a printed list of base SIMPLO CMS articles.
For this list, a user can have more views and choose one what it's convenient for him.

When you want to register your own view, it's needed to move this view file inside the `{theme}/resources/views/modules/articles_list`.
Each registered view for the `ArticlesList` module will get the following properties:

- `$articles` - a collection `Illuminate\Support\Collection` with `App\Models\Article\Article` items
- `$configuration` - a configuration `Modules\ArticlesList\Models\Configuration` model

## Image

This module provides inserting an image from [Media Library](../core/media-library.md) directly to Grid Editor. A user has
an option to set a few properties such as an alternative text or a size of this image as well.

A developer can use a custom template view for rendering all images using this module. For this action, it's needed to insert
this custom view inside the `{theme}/resources/views/modules/image` directory. This custom view **must have** `default.blade.php` name.

Example of the custom view:

```php
<?php /** @var \Modules\Image\Models\Configuration $configuration */ ?>
<figure>
    <img src="{{ $configuration->makeImageLink() }}"
         alt="{{ $configuration->alt }}"
         class="{{ $configuration->img_class }}"
         @if ($configuration->is_sized)
         style="max-width:{{ $configuration->width }}px;max-height:{{ $configuration->height }}px"
         @else
         style="width:100%"
         @endif
    >
    <figcaption>{{ $configuration->alt }}</figcaption>
</figure>
```

## Link

## Photogallery

This module offers to users inserting an existed photogallery everywhere to the website using Grid Editor. A user has an option
to choose a custom rendering view.

If you want to register a such kind of the view for this module, you must move the view file to the `{theme}/resources/views/modules/photogallery`
directory. Everything else is just simple, a user only select a view for rendering, choose a photogallery and that's all.

Example:

```php
<?php /** @var \Modules\Image\Models\Configuration $configuration */ ?>

// TODO
```

## Text

This module is really simple and just only insert some text using Grid Editor. For this module, it's not possible to register
a demarcated view (a custom rendering view). For a purpose of this module, it's not necessary at all, don't worry. You will not
need it.

This text is editable using popular WYSIWYG editor - CKEditor. As a developer, you can modify a few elements in CKEditor using 
[Theme Configuration](../theme/configuration.md).