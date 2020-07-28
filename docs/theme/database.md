---
id: database
title: Database
---

## Introduction

Laravel offers excellent interacting with a variety of databases using either **Query Builder**, powerful **Eloquent ORM** or just with 
raw **SQL commands**. By default, Laravel supports four the most uses databases. You are completely free to choose one of them from this list:
- SQLite
- SQLite Server
- PostgreSQL
- MySQL

Laravel provides [Environment Configuration](https://laravel.com/docs/5.8/configuration#environment-configuration) and because of this feature, you don't need 
to change configuration directly in `config/database.php` file. For a configuration of your database, you can use `.env` configuration file in a root directory of SIMPLO CMS. 
In this configuration file, you will find everything, what you need to set for success connection to your database and something more. 

> If you know more about **Database** in Laravel, visit an official [Laravel documentation](https://laravel.com/docs/5.8/database)

## Migrations

### Generating Migrations

In a theme's root directory, you can notice that here is located `database` directory. Out of the box, here is just only one 
directory `migrations`, what you can know from default Laravel directory structure. In a theme, this directory has the same purpose 
like in Laravel. This directory will just store all generated database migrations, which you create during development of your theme.

If you want to generate a new [database migration](https://laravel.com/docs/5.8/migrations), you can use the following 
[Artisan command](https://laravel.com/docs/5.8/artisan):
```text
$ php artisan make:migration create_your_database_table --path=/themes/Example/database/migrations
```
After running Artisan command above, a new database migration file will be created and placed in `database/migrations` directory of your theme.

### Running Migrations

When you open your migration file in `database/migrations` path of your theme, you can edit a future database table and her structure. Everything is the 
same like in Laravel and you are not restricted. After making all changes, you will probably want to run this theme's migration. It is very simple. 
You can just use command-line interface and run the Artisan command below:
```text
$ php artisan migrate --path=/themes/Example/database/migrations
```
After running this Artisan command, you can check your database. If you will see a new table there, then everything is fine!

> Unfortunately, it's **necessary** to call the Artisan commands above for generating and running migrations with a `--path` option.
> If you will **try to call this commands without this option**, a migration file will be generated to a root's directory `database/migrations` of SIMPLO CMS.

## Seeds

> **!!!! TODO: Database Seeders in theme does not work now at all! Text below will be changed !!!!**

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
$ composer dump-autoload
```

If you do everything well, 
you can use this Artisan command below for running your seeder:
```text
$ php artisan db:seed --class=Theme\YourThemeSeeder
```
After what you run this Artisan command, method `run` will be executed with your source code.