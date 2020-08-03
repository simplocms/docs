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

The default path for all migration files is `Database/Migrations` in module's directory. Every time after generating
new migrations, this files will be stored into this directory.

### Generating Migrations

For generating new migrations in the module, it's possible to use the following [Artisan Console](../core/artisan-console.md) command:

```text
$ php artisan module:make-migration create_frequently_ask_questions_table FrequentlyAskQuestion
```

How you can notice in this command above, it's just the same principle like in Laravel for generating migrations, nothing special more. Only this command has
a bit different syntax with one more required parameter - a name of the module.

### Running Migrations

After what you generated database migration and modify everything, how you need, then you will probably want to run these migrations.
For running it, then you can use the following Artisan Console command:

```text
$ php artisan module:migrate FrequentlyAskQuestion
```

> **IMPORTANT!** If you do not specify the module's name, then it will be run all migrations from all available modules.

For working with the migrations, you can also use the following Artisan Console commands:

- `module:migrate-rollback FrequentlyAskQuestion` - roll back the latest migrations in the given module

- `module:migrate-refresh FrequentlyAskQuestion` - roll back all migrations and run the `module:migrate FrequentlyAskQuestion` command then

- `module:migrate-reset FrequentlyAskQuestion` - roll back all migrations

> For **these commands above**, it is also valid when you do not specify any module's name, then the system **will run 
> everything about all available modules**.


## Seeds

The default path for seeders, it's `Database/Seeders` in module's directory. All future seeders will be stored in this place.

### Make Seeders

Generate the seeder for the specific module, you can use the following Artisan Console command:

```text
$ php artisan module:make-seed seed_fake_frequently_ask_questions FrequentlyAskQuestion
```

### Running Seeders

After what you generate the seeder and write your source code inside this seeder, run everything using this Artisan Console command below:

```text
$ php artisan module:seed FrequentlyAskQuestion
```

> **IMPORTANT!** If you do not mention the module's name, then the command runs all seeders in all available modules.