---
id: installation
title: Installation
sidebar_label: Installation
---

The latest version of Laravel used: **5.8**

## Server Requirements

Firstly make sure your server meets the following requirements:

* PHP >= 7.2
* OpenSSL PHP Extension
* PDO MySQL PHP Extension
* Mbstring PHP Extension
* Tokenizer PHP Extension
* XML PHP Extension
* MySQL database ^5.6
* Enabled mod_rewrite for Apache
* GD or Imagick PHP Extension (Imagick recommended)

For development there are few more recommended requirements that will make your work easier:

* cURL extension
* SQLite3 extension
* GD extension

> More detailed information about installation for Laravel can be found in official [Laravel documentation](https://laravel.com/docs/5.8/installation)

## Installation

If you want to setup Docker container for development, you need to install `docker-compose` and follow a guide in `.docker/README.md`.

> This installation guide is writing for "LAMP" stack environment.

If you are using project's docker image, connect to the `web` container and proceed with installation in the container:

```bash
# Run this in project root directory
$ docker-compose exec web /bin/bash
```

### Composer

> If you do not have composer on your machine, [download](https://getcomposer.org/download/) and install it. Composer is already present in Docker container.

```bash
$ composer install
```

### Node.js

> If you do not have Node.js on your machine, [download](https://nodejs.org/en/) and install it. A currently recommended version is 8.*. Node.js is already present in Docker container.

```bash
# Install npm dependencies
$ npm i
```

> **TIP for Windows users**: Run installation with `--no-bin-links` argument.

## Configuration

### File Permissions

You have to make sure, that Apache user has writing permissions to directories `bootstrap` and `storage`.

```bash
# Change owner to www-data group (apache)
$ chown -R :www-data .
# Setup permissions
$ chmod -R ug+rwx ./bootstrap ./storage
```

### Environment Configuration

```bash
# Copy preconfigured .env.example to .env
$ copy .env.example .env
# Generate application key
$ php artisan key:generate
```

> If you are using Docker container, all important configuration is setting within the container.

## Setup

At this point, database connection should be configured. If there is a problem, you will know immediately.

```bash
# Creates database tables and seeds basic data
$ php artisan migrate --seed
```

### Compile Your Assets

#### Development

In development, you want to compile assets to be debuggable and readable.

```bash
# Keep watching changes in source code and recompile on every change:
$ npm run watch
# OR single time compilation:
$ npm run dev
```

#### Production

On production server you want to compile assets to be minimized and not-readable, without debuggers and console logs:

```bash
# Build production assets:
$ npm run prod
```

### Template Installation

When you have a theme for SIMPLO CMS and you want to use it, follow the instructions below:

1) You have to copy a theme into directory `themes`.
2) If the template contains `modules` directory, go to the template's directory and run the command `composer dump-autoload`.
3) Sign in into CMS administration and go to Settings > Template (Nastavení > Šablona).
4) Click on “Change” (Změnit) and then activate your template.


### Image Optimization

Media Library can optimize uploaded PNGs, JPGs, SVGs and GIFs by running them through a chain of various image optimization tools. It will use these optimizers if they are present on the server:

- [JpegOptim](http://freecode.com/projects/jpegoptim)
- [Optipng](http://optipng.sourceforge.net/)
- [Pngquant 2](https://pngquant.org/)
- [SVGO](https://github.com/svg/svgo)
- [Gifsicle](http://www.lcdf.org/gifsicle/)

Here's how to install all the optimizers with APT:

```bash
$ sudo apt-get install jpegoptim
$ sudo apt-get install optipng
$ sudo apt-get install pngquant
$ sudo npm install -g svgo
$ sudo apt-get install gifsicle
```
