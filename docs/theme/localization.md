---
id: localization
title: Localization
---

## Introduction 

Laravel provides really simple feature of localization, which offers convenient way for supporting a lot of
languages on website. All language translations are located in `resources/lang` directory of root's SIMPLO CMS under the 
specific language code, for example the `resources/lang/en` directory contains of translation files for English.

> For more information about Laravel Localization, please visit official [Laravel Documentation](https://laravel.com/docs/5.8/localization)

## How Does It Work In Theme?

### Configuring Localization

TODO

### Define Translations

Simplo CMS is really simple CMS and everything, what developers know in Laravel, they can find the same features with the same 
way for using here. Localization is not exception. If you want to define some new localization in theme, then just create files with
key - string translations in directory with language code name, that's all. By default, in `resources/lang` directory of your theme there
are located some translation files already and they use out of the box in theme.

### Use Translations

After what you define your first translations, how developers can use them? It's the same like in Laravel, just use for example helper function
`trans` with key of specific translation, but remember, that before your specific key, you need to write theme prefix - `theme::`. The application
recognizes that this translation is located in current active theme and it will find this key inside this theme. For convenient way, better for 
using translations in theme is using method `themeTrans` (or `themeTransChoice`) of `App\Services\FrontWeb\FrontWebService`. This method adds
prefix `theme::` automatically.

For calling method `themeTrans` (or `themeTransChoice`) in controller, it's good and simple attitude, when call `getFrontWebService` method first. This method
returns `App\Services\FrontWeb\FrontWebService`, what serves `themeTrans` and `themeTransChoice` methods.

> If you want to call `getFrontWebService` method in your controller, it's **necessary** that the controller extends `App\Http\Controllers\FrontWeb\FrontWebBaseController`

If you need to use your translation inside some view, then calls `themeTrans` or `themeTransChoice` with `$_FW_SERVICE` property, which is available in all views.

> When you create your custom route, don't forget to apply middleware `front_web` for availability of property `$_FW_SERVICE`