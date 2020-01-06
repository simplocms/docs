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