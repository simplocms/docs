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

## Namespacing

When you create a new module, then the module provider registers new namespaces also corresponding with the module's name.
For `Blog` example, it's created the namespace and the key named `blog`. This namespace is required, when developers want to call
some the module's resources.

For **lang** resources just call for example `trans('blog::name.key');`.

For **view** resources just call for example `view('blog::view');`.

For **config** resources just call for example `config('blog.name.key');`.