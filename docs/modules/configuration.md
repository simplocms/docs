---
id: configuration
title: Configuration
---

## Configuration

Each module has custom configuration file named `config.php` located in `Config` of module's directory and you can set 
here a few useful configuration options.

## Available Options

#### Name

The `name` key is just only name of the module.

#### Icon

This configuration key offers you to choose some icon for your module, which will appear mainly in menu of **SIMPLO CMS** Administration.

#### For Grid Editor

The `for-grideditor` key is a boolean value and it serves for type identification of this module. Good attitude during developing
with modules is that we will have complex module without possibility to add it to the Grid Editor or just only module designed for
Grid Editor.

#### Admin

The `admin` key consists more configuration values and below, we will describe all of them.

**Menu**

The `menu` key is one of the most important configuration value, if you develops complex module with CRUD operations. This key is
array type value and it can consists one or more arrays what creates a (sub)menu.