---
id: creating
title: Creating
---

## How To Create A Module

For creating a module, developers can use simply way and they can run [Artisan Console](core/artisan-console.md) command below:

```text
php artisan module:make module-name
```

Instead of `module-name` you write a name of the module. It's highly recommended to use CamelCase convention because
this modules package uses **[PSR-4](https://www.php-fig.org/psr/psr-4/)** autoloading.

After running `module:make` Artisan command, the package will generate some resources as controllers, views, seeders and another
folders and files. Everything will appear in `modules` folder under the specific subdirectory corresponding with name of the module.