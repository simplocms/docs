---
id: compiling-assets
title: Compiling Assets
---

## Introduction

In Laravel Framework, developers use Webpack for compiling assets like CSS and Javascript files in general. It's very simple
and mainly a powerful fluent API tool, which offers a lot of options. A good message is, that you are also free to use this tool
during your work with modules. Let's try it!

> For more information about Compiling Assets (Mix), please visit an official [Laravel Documentation](https://laravel.com/docs/5.8/mix)

## How Make Assets In Module?

After when you created your module, you can create also some CSS and Javascript files in the module's `Assets` folder. If you are not comfortable with this folder, 
you are free to define your custom one. Just when you will add your asset's file to the module's `webpack.mix.js` file, instead of `Assets` you will state this customized asset folder. 
The default `package.json` file also consists of everything what you need to start. All dependencies, you can install using
the following command below:

```text 
$ cd themes/your-project-theme/Modules/Blog
$ npm install
```

## Running Mix

The `webpack.mix.js` file is a configuration file for [Webpack](https://webpack.js.org/). When you need to run your mix file with the actual tasks,
you only need to run one of the following **NPM** commands:

```text
// Ideal to use for develop environment and it will run all tasks from mix file
$ npm run dev

// Ideal to use for production environment and it will run all tasks from mix file and minify everything
$ npm run prod
```

After generating the compiled file, you will see them in `Blog/Dist` folder. Maybe now you ask yourself how you can use this files
for a public web? It's a good question and answer is - You don't need to solve it! Because during an installing process of your module, the system will
create a symlink from your module's `Dist` folder to the `public/modules/Blog` folder.

If you want to get a link for your generated asset files, you can use normally `mix` helper function with the specific a path inside the main module public folder
and as a second parameter you need to define manifest directory of your module. You can see one example below:

```php
echo mix('js/blog.min.js', 'modules/Blog');
```