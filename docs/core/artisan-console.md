---
id: artisan-console
title: Artisan Console
---

## Introduction

Artisan Console is Laravel's command-line interface, which provides a lot of commands for making work-flow more comfortable and helps
during developing application. For show all available commands use `list` command below:
```text 
$ php artisan list
```

Every command in Artisan Console offers also help information, what you can use for getting list of all available options for
specific command and more detailed descriptions. For showing this detailed information use `help`:
```text
$ php artisan help list
```

> For more information about Artisan Console, please visit an official [Laravel Documentation](https://laravel.com/docs/5.8/artisan)

## Available Artisan Console Commands In SIMPLO CMS

Including Artisan Console commands in Laravel, SIMPLO CMS offers also custom commands for specifying purposes and developers can use
them. 

**`clearcache`**

SIMPLO CMS offers custom command for global clear cache. If you want to run it, try to call the command below:

```text
$ php artisan clearcache
```
 
Instead of running `cache:clear`, `route:clear` and `view:clear` separately, you can just run `clearcache`, which already includes calling these
Artisan Console commands. 

**`fix:paths`**

This command fixes symlinks of all installed modules and a default theme.

```text
$ php artisan fix:paths
```

This command is convenient to run after localization changing of your project. 

**`migrate:modules`**

This command runs database migrations for all installed modules.

```text
$ php artisan migrate:modules
```

It's recommend to run immediately after `php artisan migrate`.

**`migrate:urls {url} {replacement} {--I|info}`**

This command replaces all specified url addresses by a new one. Be attention that this command does not make a validation of
these url addresses (because of more reasons). It's necessary to be careful and before running this command with replacing,
it's convenient to run this command with --I or --info parameter option for printing only information about the future replacements.
After checking that everything is fine, you can run this command without this optional parameter.

```text
$ php artisan migrate:urls {url} {replacement} {--I|info}
```

The convenient steps are the following:

1. php artisan migrate:urls ://old-domain.cz ://new-domain.cz -I
2. Checking printed information in a command line
3. php artisan migrate:urls ://old-domain.cz ://new-domain.cz

**`module:install {module*}`**

With this command, you can install one or more modules in the same time.

```text
$ php artisan module:install {module*}
```

For example: `php artisan module:install Image Link Text` - it will be installed `Image`, `Link` and `Text` modules.

**`run-tests {--without-tty : Disable output to TTY}`**

Using this command you can run all implemented tests. For running tests, it's recommend this command because of correct
configuration of environment, clear cache, set a few configurations, etc. 

```text
$ php artisan run-tests {--without-tty : Disable output to TTY}
```

This command accepts the same parameters like `phpunit`. The given parameters are transmitted to `phpunit`, which will
call with this command.

For running only specific test, run this command: `php artisan run-tests --filter=ArticlesTest`

> Because of limits in an implementation of this command, we **don't recommend strongly cancel processing of tests** before their finishing!

**`update:run {name}`**

TODO!!!