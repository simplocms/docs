---
id: compiling-assets
title: Compiling Assets
---

## Introduction

Laravel provides a simple fluent API, what developers can use for specification Webpack building through a lot of CSS and
Javascript pre-processors. Of course, if you don't want to use Webpack, you can choose another tool for developing your application or
just use nothing, it's your decision.

> For more information about Compiling Assets (Mix), please visit an official [Laravel Documentation](https://laravel.com/docs/5.8/mix)

## Installation And Definition

In SIMPLO CMS theme, working with Laravel Mix is still the same how you know it from Laravel. By default, in `webpack.mix.json` file,
which is located in root's directory of a theme, there is a source code already for compilation assets and for importing a few libraries, plugins and their settings. 
If your theme requires more, you can change this source code how you need.

For compiling assets using Webpack, first you need to **install Laravel Mix** in your theme's root directory. The default Example theme already consists of a `package.json` file with a few 
libraries, which are important for running SIMPLO CMS theme. If you created your theme with copying this default theme, then all things are prepared. 
For running of the installation process, you need to run the following command in command-line:

```text
$ npm install
```

After the installation process, in a theme's root directory will be appeared the `node_modules` directory. When everything is fine, you can 
run the compiling process using Webpack. Just run one of the following command using command-line:

```text
// For run Mix in develop environment
$ npm run dev

// For run Mix in production environment (with minify output)
$ npm run prod
```

For watching assets for the future changes, then run the following command:

```text
$ npm run watch
```

When you want to use [Versioning](https://laravel.com/docs/5.8/mix#versioning-and-cache-busting) in your theme, you can do 
everything, how you know from Laravel as well. If you need to get url for actual version file of some asset, then use instead of `mix` Laravel helper function
`themeMix` method of `App\Services\FrontWeb\FrontWebService`, which runs everything for correct returning the theme's version file of the specific asset. For getting url
path to assets without versioning feature, then use `themeAsset` method of `App\Services\FrontWeb\FrontWebService`.

For example:

**layouts/main.blade.php**
```html
<?php
/** @var \App\Services\FrontWeb\FrontWebService $_FW_SERVICE */
/** @var \App\Services\FrontWeb\FrontWebData $_FW_DATA */
?>
<!DOCTYPE html>

<!--[if lte IE 8]> <html class="no-js old-ie" lang="en"> <![endif]-->
<!--[if gt IE 8]><!-->
<html class="no-js" lang="{{ $_FW_SERVICE->getContentLanguage()->language_code }}">
<!--<![endif]-->

<head>
    <title>{{ $_FW_DATA->getTitle() }}</title>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="author" content="SIMPLO s.r.o.">

    {{-- AUTOMATIC ICONS --}}
    {!! \App\Facades\ViewComponent::insert(\App\Components\Views\SiteIconsView::class) !!}

    {{-- AUTOMATIC META TAGS --}}
    {!! \App\Facades\ViewComponent::insert(\App\Components\Views\SiteCommonMetaView::class) !!}

    <script>
        document.documentElement.className = document.documentElement.className.replace("no-js", "js");
    </script>

    {{ Html::style($_FW_SERVICE->themeMix('css/main.css')) }}

</head>
<body>

    {{-- Primary Menu --}}
    @viewComponent(\Theme\Components\View\PrimaryMenu)
    {{-- /Primary Menu --}}

    @yield('content')

    {{-- STRUCTURED DATA FOR RICH SNIPPETS --}}
    {!! $_FW_DATA->getStructuredData() !!}

    {{ Html::script($_FW_SERVICE->themeMix('js/app.js')) }}

    {!! ViewComponent::insert(\App\Components\Views\GATrackingCodeView::class) !!}

    @stack('scripts')

</body>
</html>
```

How you can notice in `<head>` element, there is included `css/main.css` file by `themeMix` method. This method mainly set `theme` as
a manifest directory during getting the asset with `Mix` instance. Then you do not need to do it manually. 
If you are interesting where this directory is located, check a public directory in root's SIMPLO CMS. You can find a symlink to your theme's `resources/dist` 
directory there.