---
id: views
title: Views
---

## Introduction

Views provides you a good way for creating presentation documents (mainly HTML), what are separated from logic side the most 
often defined in controllers. By default, Laravel stores all views in `resources/views` directory.

> For more details, you can visit official [Laravel documentation](https://laravel.com/docs/5.8/views)

## How To Create Views

Views in theme is the same like in Laravel. Actually, the view store is also the same and you will find it in `resources/views` in 
theme's root. When you will visit this path with views in first time, there are already a few view folders and files. All there is 
a layout for basic pages and parts of these pages and you are free to change them how you want and need.

## Describe Default Views

In the last section of this documentation page, we met with creating views for theme. How you also know, in `resources/views` path 
are some default view files in first time.

The default structure of views directory looks like this:
```text
resources/views
├── admin
|   └── _settings_form.blade.php
├── articles
|   └── detail.blade.php
├── homepage
|   └── index.blade.php
├── layouts
|   └── main.blade.php
├── menus
|   └── primary.blade.php
├── modules
|   └── articles_list
|       └── list.blade.php
├── pages
|   ├── articles.blade.php
|   ├── page.blade.php
|   └── search.blade.php
└── vendor
    └── breadcrumbs.blade.php
```