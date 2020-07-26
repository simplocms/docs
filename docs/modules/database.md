---
id: database
title: Database
---

## Introduction

By default, [Modules](modules/general.md) package offers the same concept for a database like Laravel. The default paths
about Migrations and Seeders are just
`Database/Migrations` and `Database/Seeders` folders.

> For more information about a work with a database, please visit first an official [Laravel Documentation](https://laravel.com/docs/5.8/database).

## Migrations

The default path for all migration files is `Database/Migrations` in module's directory. Every time during generating
new migrations, it will be stored into this directory.

### Generating Migrations

For generating new migrations in the module, it's possible to use the following [Artisan Console](../core/artisan-console.md) command:

```text
$ php artisan module:make-migration create_blog_posts_table Blog
```

How you can notice in this command above, it's just the same principle like in Laravel for generating migrations, nothing special more. Only this command has
a little bit different syntax with more required parameters.

### Running Migrations

After what you generated database migration and modify everything, how you need, then you will probably want to run these migrations.
For running it, then you can use the following Artisan Console command:

```text
$ php artisan module:migrate Blog
```

> **IMPORTANT!** If you don't specify the module's name, then it will be run all migrations from all available modules.

## Seeds

The default path for seeders, it's `Database/Seeders` in module's directory. All future seeders will be stored in this place.

### Make Seeders

Generate the seeder for the specific module, you can use the following Artisan Console command:

```text
$ php artisan module:make-seed seed_fake_blog_posts Blog
```

### Running Seeders

After what you generate a seeder and write your source code inside this seeder, run everything using this Artisan Console command below:

```text
$ php artisan module:seed Blog
```

> **IMPORTANT!** If you don't mention the module's name, then such a command runs all seeders in all available modules.