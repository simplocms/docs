---
id: setting
title: Setting
---

## Introduction

When you create your custom theme, sometimes you can need to have some custom setting in [Administration Panel](../getting-started/setting.md).
If this time is coming now, read the following paragraphs how you can make them.

## How To Make Theme Setting

When you will start to develop your theme using copying from Example theme, by default there will be `admin/_settings_form.blade.php` view.
It's an entry point where you can define all possible theme's settings and extend default SIMPLO CMS settings. If you will open this view file,
you can see something like below:

```html 
<div class="col-xs-12">
    <v-form-group :error="form.getError('{{ $articlesPageField }}')">
        <label for="input-theme-articles-page-id">{{ trans('theme::config.form_labels.articles_page_id') }}</label>
        {{ Form::select($articlesPageField, $pages, null, [
            'class' => 'form-control',
            'v-model' => "form.$articlesPageField"
        ]) }}
    </v-form-group>
</div>
```

Probably you can understand this source code, but you can be also a bit confused where this view is rendered. For rendering this view, it's
using `Theme\Components\SettingsFormFactory` component with `createView` method, which implements `App\Services\ThemeService\Contracts\ThemeSettingsFormFactoryInterface` interface. 
By default, there are loaded all available pages for a current language. Then these pages are possible options for selecting of an article page, where will be loaded all articles (list of articles).

> The `Theme\Components\SettingsFormFactory` component **is bind to SIMPLO CMS** using `Theme\Providers\ThemeServiceProvider`.

Now you know where you can define your new custom setting inputs. But what about validation rules? It's possible to define in
`getValidationRules` method of `Theme\Components\SettingsFormFactory` component. By default, here is already one validation rule
for the article page select input:

```php
<?php declare(strict_types = 1);
     
namespace Theme\Components;

use App\Models\Page\Page;
use App\Models\Web\Language;
use App\Services\ThemeService\Contracts\ThemeSettingsFormFactoryInterface;
use App\Services\ThemeService\Theme;
use Illuminate\Contracts\View\View;
use Illuminate\Validation\Rule;
use Illuminate\View\Factory as ViewFactory;
 
final class SettingsFormFactory implements ThemeSettingsFormFactoryInterface {
 
...

    /**
     * Get theme settings validation rules.
     *
     * @param \App\Models\Web\Language $language
     * @param \App\Services\ThemeService\Theme $theme
     * @return mixed[]
     */
    public function getValidationRules(Language $language, Theme $theme): array
    {
        return [
            $theme->getSettingsKey('%lang%_articles_page_id', $language->language_code) => [
                'nullable',
                'int',
                Rule::exists(Page::getTableName(), 'id')
                    ->where('language_id', $language->getKey())
            ]
        ];
    }

...
```

You can notice, that it's completely the same concept like in Laravel. If you know well Laravel Validation, then you will not have
any troubles with definition validation rules for your theme's settings. Just only for input name (key), you need to get correct
theme's setting key and for this purpose, it's available on `$theme` instance already `getSettingsKey` method. Really, for definition of
validation rules, it's not necessary to do anything special.

For correct storing settings with also your custom theme's settings, you need to make last changes in `admin.php` config file of your theme.
Inside this config file, you can see the `settings` key and by default, here is defined `%lang%_articles_page_id` key as an integer type.
It's a place where you must define all your custom settings which you want to store to a database. A key value format is the following:

```php
<?php

return [

    'settings' => [
        '%lang%_articles_page_id' => \App\Services\Settings\SettingsService::TYPE_INT
    ],

];
```

> For a **multilanguage setting**, you must define your key with `%lang%` wildcard.

Under the key, here is a name of your custom setting, which will be stored also in a database (settings table), and which will be as an identifier everywhere
in your application for getting the value. For getting the value, you can use the following code inside a theme's controller:

```php
$this->themeSettingsService->getInt('%lang%_articles_page_id');
```

> Using **Dependency Injection**, you can have available `\App\Services\ThemeService\ThemeSettingsService` instance everywhere you need.

As a value for the key in `admin.php` theme's config file, you can define a type for correct casting. In SIMPLO CMS, you can use the following
types:

- `TYPE_INT` - for an integer
- `TYPE_BOOL` - for a boolean
- `TYPE_MEDIA_FILE` - for a media file
- `TYPE_IMAGE` - for an image
- `TYPE_DICTIONARY` - TODO !!!

When you do not need to specify any type from list above, you can define type as a null value (or empty string, it's up to you). A default value of the type is
a string.

If you will not need to define the type for your all custom settings (the type is just a string by default), you can define your custom settings as an indexed array. Let's see
the example below:

```php
<?php

return [

    'settings' => [
        'one_setting_key', 'second_setting_key', 'third_setting_key'
    ],

];
```

After the steps above, your custom settings will be ready for use, go to try it!