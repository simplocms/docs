---
id: compiling-assets
title: Compiling Assets
---

## Introduction

Laravel provides a simple fluent API, what developers can use for specification Webpack building through a lot of CSS and
Javascript pre-processors. Of course, if you don't want to use Webpack, you can choose another tool for developing your application or
just use nothing, it's your decision.

> For more information about Compiling Assets (Mix), please visit official [Laravel Documentation](https://laravel.com/docs/5.8/mix)

## Compiling Assets With Theme

In SIMPLO CMS theme, the working with Laravel Mix is still the same how you know it from pure Laravel. By default, in `webpack.mix.json` file,
which is located in root's directory of theme, there is a source code already for definition main assets and setting. If your theme requires more, you can
change or increase this source code how you need.

When you want to use [Versioning](https://laravel.com/docs/5.8/mix#versioning-and-cache-busting) in your theme, you can do 
everything how you know from pure Laravel too. But when you need to get url for actual version file of some asset, then use instead of `mix` helper function
`themeMix` method of `App\Services\FrontWeb\FrontWebService`, which runs everything for correct returning theme's version file of the specific asset. For getting url
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