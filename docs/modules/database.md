---
id: database
title: Database
---

## Introduction

By default, [Modules](modules/general.md) package offers the same concept for database like Laravel. The default paths
about [Migrations](https://laravel.com/docs/5.8/migrations) and [Seeders](https://laravel.com/docs/5.8/seeding), you can set
for new generated modules in [Configuration](../getting-started/configuration) file named `config/modules.php` - exactly 
under the `paths.generator` key.

> For more information, please visit official [Laravel Documentation](https://laravel.com/docs/5.8/database).

## Migrations

The default path for all migration files is `Database/Migrations` in module's root directory. Every time during generating
new migrations, it will be stored into this directory.

### Generating Migrations

For generating new migrations in the module, it's possible to use the following [Artisan Console](https://laravel.com/docs/5.8/artisan) command:

```text
php artisan module:make-migration create_blog_posts_table Blog
```

How you can notice to this command above, it's just the same principle like in Laravel for generating migrations, nothing more. Only this command has
a little bit different syntax with more parameters.