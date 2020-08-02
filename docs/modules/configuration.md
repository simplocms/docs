---
id: configuration
title: Configuration
---

## Configuration

Each module has custom configuration file named `config.php` located in `Config` of module's directory. You can set 
here a few useful configuration options.

## Available Options

#### Name

The `name` key is just only name of the module. For example `FrequentlyAskQuestion`.

#### Icon

This configuration key offers you to choose some icon for your module, which will appear mainly in the menu of **SIMPLO CMS** Administration. This icon
is CSS class of Font Awesome library. It is not necessary to state `fa-` prefix. A default value of this icon is `window-restore`.

#### For Grid Editor

The `for-grideditor` key is a boolean value. It serves for type identification of this module. Good attitude during developing
with modules is that we will have complex module without possibility to add it to the Grid Editor or just only module designed for
Grid Editor. A default value is true, so it means when you do not specific this config key, your module will be possible to use only in Grid Editor.

The `grideditor_title` key is a string value, where the developer can define title directly in string or just use a translation key. The system will check first
if the defined value is a translation key and if it is not, this value will be printed without some changes.

> **NOTICE** If you will decide to use **a translation key** from your module, you **must specify also a namespace**! For example: `module-articleslist::admin.grideditor_title`

#### Admin

The `admin` key consists more configuration values and below, we will describe all of them.

**Menu**

The `menu` key is one of the most important configuration value, if you develop a complex module with editable items (not designed for Grid Editor). This key is
an array type value, which can consist one or more arrays what creates a (sub)menu.

If you want to config your custom simple menu, it is real simple. In `admin.menu` key, add translation key with an array. This translation
key must be placed in `admin.menu` translation key of the current module if you want to have correct translation for this menu item. For this array in
`admin.menu` config key, you must define `route` with the route name as a value. The second optional parameter is `icon` what you can use here, but for
Font Awesome icon, you must write a complete CSS class - for example `fa fa-comments`.

If you want to config your custom menu with more items, you can use the same steps like for the simple menu. Just only for the `admin.menu` key in configuration file,
you will specific more items here. The best result for an admin submenu is when you will not use `icon` optional parameter for these submenu items separately and just only define one group icon,
which will generate for the main item of this submenu in first level of the admin menu. For definition of this group icon, you need to use `admin.menu_group_icon` config key and,
the same like in menu items, you need to define a complete CSS class for Font Awesome icon. For this group icon, a default value is `fa fa-trello`. You will also want to set certainly a group text
of the submenu. If yes, then open `admin.php` translation file in a `Resources/lang/"lang_code"` directory of your module, and define `menu.group_text` key.

**Permissions**

The another important option is `permissions` key. It is an array type value, where you can define here a group type for permission as an `admin.permissions.group` key. For this option, you
can use pre-defined value, which you can find in **permissions.php** config file located in SIMPLO CMS `config` folder. For the complex module with editable items, the best option is just `1`, because then the system will use 
a basic logical entrusting. It means with the following areas - show, create, edit, delete, all.

## Example: Config and Translation

```php 
<?php

return [
    'name' => 'ModuleName', // module name
    'icon' => 'icon',       // module icon - font awesome class without 'fa-' prefix

    'for-grideditor' => false, // designed for Grid Editor

    // admin configuration options
    'admin' => [
        'menu_group_icon' => 'fa fa-icon',                      // admin menu group icon
        'menu' => [                                             
            'first' => [                                        // first submenu in admin menu (`first` is a translation key corresponding with 'module-module_name::admin.menu.first')
                'route' => 'module.module_name.first.index',    // first submenu route name
            ],
            'second' => [
                'route' => 'module.module_name.second.index',
            ]
        ],
        'permissions' => [
            'group' => 1        // permission group type - '1' means the permission group with the following areas: show, create, edit, delete, all
        ],
    ]
];
```
*"module_path"/Config/config.php*

```php 
<?php

return [

    'menu' => [
        'group_text' => 'Name of your module in admin menu - menu with more items',

        'first' => 'First menu item',
        'second' => 'Second menu item'
    ],

    'permissions' => 'Name of your module'

];

```
*"module_path"/Resources/lang/en/admin.php*