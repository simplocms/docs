---
id: models
title: Models
---

## Introduction

Laravel includes amazing Eloquent ORM, what is simple ActiveRecord implementation for working with your database and her
tables. For using this principle of communication with your database, you have to make **Model**. Then, you can use this model and 
simply insert new records or make some updates of records in specific database table, which is connected with your model.

> For more details, go to read official [Laravel documentation](https://laravel.com/docs/5.8/eloquent)

## How Does It Work In Theme?

If you met with Laravel Eloquent ORM and now you know how can you define models in Laravel, you don't need to know something more! Just you will 
move to src directory in theme's root and make new models folder here. Then in models directory, you can just create new model file and 
insert code below:
```php
<?php declare(strict_types = 1);

namespace Theme\Models;

use Illuminate\Database\Eloquent\Model;

class YourModel extends Model
{
    
    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [];

    /**
     * The table associated with the model.
     *
     * @var string
     */
    protected $table = 'your_table_name';

}
```
**It is important to say** that your model must extends from `Illuminate\Database\Eloquent\Model` class for possible corresponding with some database 
table.

> It is not necessary to keep directory of models like in the example above. You are completely free to make your custom directory for all models of your theme, 
> where you want.