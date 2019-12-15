---
id: database
title: Database
---

## Introduction

Laravel offers excellent interacting with a variety of databases using either **query builder**, powerful **Eloquent ORM** or just with 
raw **SQL commands**. By default, Laravel supports four the most uses databases and you are completely free to choose one of them from this list:
- SQLite
- SQLite Server
- PostgreSQL
- MySQL

For choose one of them, you can use `config/database.php` configuration file in root directory. In this database configuration file, 
you will find everything, what you need to set for success connection to your database and something more. Laravel providers 
[environment configuration](https://laravel.com/docs/5.8/configuration#environment-configuration) here too and because of this feature, you don't need to change configuration directly in `config/database.php` file 
and you can do everything in your `.env` file, what is located in root directory.

> If you know more about **Database** in Laravel, visit official [Laravel documentation](https://laravel.com/docs/5.8/database)

## Migrations

### Generating Migrations

In theme's root directory, you can notice that here is located `database` directory. Out of the box, here is just only one 
directory `migrations`, what you can know from default Laravel directory structure. In theme, this directory has the same purpose 
like in Laravel. This directory will just store all generated database migrations, which you create during development in theme.

If you want to generate new [database migration](https://laravel.com/docs/5.8/migrations), you can use the following 
[Artisan command](https://laravel.com/docs/5.8/artisan):
```text
php artisan make:migration create_your_database_table --path=/themes/Example/database/migrations
```
After running Artisan command above, new database migration file will create and place in `database/migrations` directory of your theme.

### Running Migrations

When you open your migration file in `database/migrations` path of your theme, you can edit future database table and her structure. Everything is the 
same like in Laravel and you aren't restricted. After making all changes, you will probably want to run this theme's migration. It is very simple 
and you can use command-line interface and run Artisan command below:
```text
php artisan migrate --path=/themes/Example/database/migrations
```
After running this Artisan command, you can check your database and you will see new table there. Everything is fine!

## Seeds

> **!!!! WARNING !!!! TODO: Database Seeders in theme does not work now at all!
> Text below will be changed**

### Make Seeders

If you want to write new [database seeder](https://laravel.com/docs/5.8/seeding), unfortunately you need to create seeder and save it **manually** now. The best place for depository 
of custom seeders in theme is certainly make new directory named `seeds` in `database` of your theme. After that, you can create new seeder file 
here and copy the following source code:
```php
<?php

namespace Theme;

use Illuminate\Database\Seeder;

class YourThemeSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        // TODO: It is place for your source code
    }
}
```

### Running Seeders

Then, you insert your source code to the `run` method instead of TODO comment, which you want this seeder runs. Before than you 
try to run this seeder, you have to need to regenerate Composer's autoloader. For this action, use:
```text 
composer dump-autoload
```

If you do everything well, 
you can use this Artisan command below for running your seeder:
```text
php artisan db:seed --class=Theme\YourThemeSeeder
```
After what you run this Artisan command, method `run` will be executed with your source code.