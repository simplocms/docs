---
id: artisan-console
title: Artisan Console
---

## Introduction

Artisan Console is Laravel's command-line interface, which provides a lot of commands for making work-flow more comfortable and helps
during developing application. For show all available commands use `list` command below:
```text 
php artisan list
```

Every command in Artisan Console offers also help information, what you can use for getting list of all available options for
specific command and also more detailed descriptions. For showing this detailed information use `help`:
```text
php artisan help list
```

> For more information about Artisan Console, please visit official [Laravel Documentation](https://laravel.com/docs/5.8/artisan)

## Available Artisan Console Commands In SIMPLO CMS

Including Artisan Console commands in Laravel, SIMPLO CMS offers also custom commands for specifying purposes and developers can use
them. For example, SIMPLO CMS offers custom command for global clear cache and it's run after using this command below:
```text
php artisan clearcache
```

Instead of running `cache:clear`, `route:clear` and `view:clear` separately, you can just run `clearcache`, which already includes calling these
Artisan Console commands mentioned in this sentence. 

For getting more SIMPLO CMS Artisan Console commands, just run `list` command.