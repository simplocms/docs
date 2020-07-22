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

This configuration key offers you to choose some icon for your module, which will appear mainly in menu of **SIMPLO CMS** Administration. This icon
is css class of Font Awesome library and it is not neccesary to state `fa-` prefix.

#### For Grid Editor

The `for-grideditor` key is a boolean value and it serves for type identification of this module. Good attitude during developing
with modules is that we will have complex module without possibility to add it to the Grid Editor or just only module designed for
Grid Editor.

#### Admin

The `admin` key consists more configuration values and below, we will describe all of them.

**Menu**

The `menu` key is one of the most important configuration value, if you develop complex module with editable items. This key is
array type value and it can consists one or more arrays what creates a (sub)menu.

If you want to config your custom simple menu, it is real simple. In `admin.menu` key, add translation key with an array. This translation
key must be placed in `admin.menu` translation key of the current module if you want to have correct translation for this menu item. For this array in
`admin.menu` config key, you must define `route` with the route name as a value. The second optional parameter is `icon` what you can define here, but for
Font Awesome icon, you must define a complete css class - for example `fa fa-comments`.

If you want to config your custom menu with submenu, you can use the same steps like for the simple menu. Just only for the `admin.menu` key in configuration file,
you will define more items here. The best result for an admin submenu is when you will not use `icon` optional parameter for these subitems and just only define a group icon,
which will generate for the main item of this submenu in first level of the admin menu. For definition of this group icon, you need to use `admin.menu_group_icon` config key and,
the same like in menu items, you need to define a complete css class for Font Awesome icon.

**Permissions**

The another important option is `permissions` key. It is an array type value and you can define here a group type for permission as an `admin.permissions.group` key. For this option, you
can use pre-defined value, which you can find in **permissions.php** config file. For the complex module with editable items, the best option is just `1`, because then the system will use 
a basic logical entrusting. It means the following areas - show, create, edit, delete, all.