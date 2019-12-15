---
id: controllers
title: Controllers
---

## Introduction

Controllers offer you to handle all of your requests in one class as methods instead of Closure in route files. It is correct way 
for clean and robust code. By default, Laravel stores controllers in `app/Http/Controllers` path. 

> For more details, go to read official [Laravel documentation](https://laravel.com/docs/5.8/controllers)

## How To Define Controllers In Theme

In theme, you can also choose controllers for handling all of requests and store them to `src/Http/Controllers` path of 
your theme. For the controller in theme is proper, when extends `App\Http\Controllers\FrontWeb\FrontWebBaseController`, what is the base 
controller in SIMPLO CMS stored in `app/Http/Controllers/FrontWeb` path in root directory and provides another convenient
methods, which helpful during theme's development such as the `themeView` method, which make view content with prefix `theme::` for getting 
theme view simply.

After that you created successfully your new controller, then it is necessary to reload Composer's autoloader with command below:
```text 
composer dump-autoload
```

It should be noted that there are already several controllers in theme controllers directory - `ArticlesController`, `PagesController` and 
`SearchController`. These controllers provide you a good way for modify some default logic or view for base pages in SIMPLO CMS.