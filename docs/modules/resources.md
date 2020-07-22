---
id: resources
title: Resources
---

## Introduction

Resources are loading automatically using the module's service provider. When you create a module using `module:make` Artisan Console
command, all code for this purpose will be generated in the module's service provider. Better way for making a new module, you can just
copy already some existed module and modify everything how you need.

## Configuration

For example, a main registered configuration, which is located at module's service provider, has the following source code in a `registerConfig` method: 

```php
<?php

    ...    

    /**
     * Register config.
     *
     * @return void
     */
    protected function registerConfig()
    {
        $this->mergeConfigFrom(
            __DIR__.'/../Config/config.php',
            $this->namespace
        );
    }

    ...
```

This code does nothing special - just only merge config from the main module's configuration with the others.
If you need to change something here for something more specific, you are free to do it. But anyway, this source code will be convenient for
the majority of the modules, what you will create for your application.

## Views

The module's resources of views are loaded in the module's service provider too:

```php
<?php

    ...    

    /**
     * Register views.
     *
     * @return void
     */
    public function registerViews()
    {
        $this->loadViewsFrom(
            __DIR__.'/../Resources/views',
            $this->namespace
        );
    }

    ...
```

Everything is very similar like in configuration section. Just call method for loading of the module's
view resources and that's all. When you will have to change something because of your project, you can do it right here.

## Language Files

```php
<?php

    ...    

    /**
     * Register translations.
     *
     * @return void
     */
    public function registerTranslations()
    {
        $this->loadTranslationsFrom(__DIR__ .'/../Resources/lang', $this->namespace);
    }

    ...
```

It's code for loading the module's language files. If you need to change something, you are free to do it directly in the module's
service provider.

## More Information

How you could notice in the examples above, everywhere is used the instance `$namespace` property. For sure, it is important to modify if
you created your new module using copying from an already existed module. The namespace has to start with the `module-` prefix for the correct identification of the module
through the system.

If you need to get more information about this topic, please visit [Official Package Documentation](https://nwidart.com/laravel-modules/v3/advanced-tools/module-resources)
