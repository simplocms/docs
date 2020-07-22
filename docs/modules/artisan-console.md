---
id: artisan-console
title: Artisan Console
---

## Introduction

Artisan Console is Laravel's command-line interface, which allows to run a lot of useful commands.

> For more information about Artisan Console, please visit [Artisan Console](../core/artisan-console.md) section

## Available Artisan Console Commands For Working With Modules

### Utilities

**`module:make`**

The `module:make` command run to creating process of a new module.

```text
php artisan module:make module-name
```

**`module:list`**

The `module:list` command shows all available modules.

```text
php artisan module:list
```

**`module:migrate`**

The `module:migrate` command migrates all actual database migration files. If it's not specified module name, then 
system runs all database migrations from all modules.

```text
php artisan module:migrate module-name
```

**`module:migrate-rollback`**

The `module:migrate-rollback` command rollbacks module's migrations. If it's not specified module name, then 
system rollbacks all modules.

```text
php artisan module:migrate-rollback module-name
```

**`module:migrate-refresh`**

The `module:migrate-refresh` command refreshes module's migrations. If it's not specified module name, then 
system refreshes all modules.

```text
php artisan module:migrate-refresh module-name
```

**`module:migrate-reset`**

The `module:migrate-reset` command resets module's migrations. If it's not specified module name, then 
system resets all modules.

```text
php artisan module:migrate-reset module-name
```

**`module:seed`**

The `module:seed` command seeds all available database seeds. If it's not specified module name, then system runs all
database seeds from all modules.

```text
php artisan module:seed module-name
```

**`module:update`**

The `module:update` command updates the given module.

```text
php artisan module:update module-name
```

### Generators

**`module:make-command`**

The `module:make-command` command creates a command.

```text
php artisan module:make-command command-name module-name
```

**`module:make-migration`**

The `module:make-migration` command creates a database migration file.

```text
php artisan module:make-migration migration-name module-name
```

**`module:make-seed`**

The `module:make-seed` command creates a database seed file.

```text
php artisan module:make-seed seed-name module-name
```

**`module:make-controller`**

The `module:make-controller` command creates a controller.

```text
php artisan module:make-controller controller-name module-name
```

**`module:make-model`**

The `module:make-model` command creates a model.

```text
php artisan module:make-model model-name module-name
```

## Another Commands For Modules

It's not everything! The module package offers more Artisan Console commands than we described above on this page. You can
also use commands for generate middleware, email, provider, resource, event, listener, job and more. For getting more
information, please visit [Official Package Documentation](https://nwidart.com/laravel-modules/v3/advanced-tools/artisan-commands).

## How To Create Custom Command For Module

If you have interest to create your custom command, use the following command:
```text  
php artisan module:make-command command-name module-name
```

For example, when you run this command above for module Blog, class CommandName will appear as `Modules/Blog/Console/CommandName` in
module's directory.

After when you generate your custom command, then you have to register it. For this purpose, it's possible to use method `commands`
inside the module's provider class:

```php
<?php

$this->commands([
    \Modules\Blog\Console\CreatePostCommand::class,
]);
```

When you generate and register a new custom command, it's available already for `php artisan` in the console.