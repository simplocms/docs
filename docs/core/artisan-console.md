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
specific command and more detailed descriptions. For showing this detailed information use `help`:
```text
php artisan help list
```

> For more information about Artisan Console, please visit official [Laravel Documentation](https://laravel.com/docs/5.8/artisan)

## Available Artisan Console Commands In SIMPLO CMS

Including Artisan Console commands in Laravel, SIMPLO CMS offers also custom commands for specifying purposes and developers can use
them. 

**`clearcache`**

SIMPLO CMS offers custom command for global clear cache. If you want to run it, try to call the command below:

```text
php artisan clearcache
```
 
Instead of running `cache:clear`, `route:clear` and `view:clear` separately, you can just run `clearcache`, which already includes calling these
Artisan Console commands. 

**`fix:paths`**

During working with a theme, it can appear a problem with a public link of your active theme sometimes. If you will notice, that it's your case, you don't need to solve
it by yourself. You can just call the following command below and everything will be fine:

```text
php artisan fix:paths
```

For getting more SIMPLO CMS Artisan Console commands, just run `list` command.