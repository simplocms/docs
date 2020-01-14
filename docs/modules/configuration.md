---
id: configuration
title: Configuration
---

## Configuration

When you check the main config directory in SIMPLO CMS, here is already `modules.php` configuration file. The file providers
some configuration options and you are free to modify them, how it will be convenient for you.

## Available Options

### Namespace

Default namespace for all modules.

### Stubs

Default stubs, which will be generated for modules. You can use them for specify custom output for files.

### Paths

It's place for settings default paths of generated modules. You can set the following options:

#### Modules

The path for storing all generated modules.

#### Assets

The path, where will appear all compiled assets.

#### Migration

The path, where will be published all  modules' migrations after running [Artisan Console](core/artisan-console.md) 
command `module:publish-migration`.

#### Generator

It's the most powerful option, where you can customize the module's directory structure. Here is place for definition of custom 
directories for asset sources, config files, database migrations, events, listeners, models, views, lang and etc.

### Scan

The package will be scanned specific path, what it's defined here. It's useful for the package, which is hosted in packagist website.