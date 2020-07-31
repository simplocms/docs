---
id: localization
title: Localization
---

## Introduction 

Laravel provides really simple feature of localization, which offers convenient way for supporting a lot of
languages on website. All language translations are located in `resources/lang` directory of root's SIMPLO CMS under the 
specific language code, for example the `resources/lang/en` directory contains of translation files for English.

> For more information about **Laravel Localization**, please visit an official [Laravel Documentation](https://laravel.com/docs/5.8/localization)

## How Does It Work In Theme?

### Define Translations

If you want to define some new localization in a theme, then just create files with key - string translations in a directory 
with language code name, that's all. By default, in `resources/lang` directory of your theme there are located some translation 
files out of the box.

### Use Translations

After what you define your first translation, how developers can use them? Just use for example Laravel helper function
`trans` with key of specific translation, but remember, that before your specific key, you need to write theme prefix - `theme::`. The application
recognizes that this translation is located in a current active theme. For convenient way, better for using translations in a theme is using 
method `themeTrans` (or `themeTransChoice`) of `App\Services\FrontWeb\FrontWebService`. This method adds prefix `theme::` automatically.

For example, in a theme's controller it's possible to use the following getter:

```php
$this->getFrontWebService()->themeTrans('articles.title');
```

> If you want to call `getFrontWebService` method in your controller, it's **necessary** that the controller extends the `App\Http\Controllers\FrontWeb\FrontWebBaseController` controller.

If you need to get a translation inside a view, then use the following source code:

```php
$_FW_SERVICE->themeTrans('articles.title');
```

> When you create your custom route, don't forget to apply middleware `front_web` for availability of property `$_FW_SERVICE`.