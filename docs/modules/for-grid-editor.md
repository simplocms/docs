---
id: for-grid-editor
title: For Grid Editor
---

## Introduction

In [Modules / Complete Guide](complete-guide.md), you can get an information about implementing modules for a new entity.
If you want to extend directly Grid Editor about new possible module, this is the right page where you can learn it.

## First Steps

For the first step, it is necessary to make a new module's directory. It's simple more than for a module with entity, because 
all SIMPLO CMS default modules are designed for Grid Editor only. It means that is all one which one default module you will
choose for copying and starting position for your new Grid Editor designed module.

Let's make a copy from some SIMPLO CMS default module and removed directories / files for the same directory structure like
on the scheme below:

```text
.
├── Assets
|   └── js
|       └── configuration.js
├── Config
|   └── config.php
├── Database
|   └── Migrations
├── Dist
├── Http
|   ├── Controllers
|   |   └── ModuleController.php
|   ├── Requests
|   |   └── ModuleRequest.php
|   └── routes.php
├── Models
|   ├── Configuration.php
|   └── GridEditorForm.php
├── Providers
|   └── ServiceProvider.php
├── Resources
|   ├── lang
|   |   └── en
|   |       └── admin.php
|   └── views
|       ├── configuration
|       |   └── form.blade.php
|       ├── missing_view.blade.php
|       └── module_preview.blade.php
├── .gitignore
├── composer.json
├── module.json
├── package.json
├── README.md
├── start.php
└── webpack.mix.js
```

## How To Make - Database, Models, Controllers, Compiling Assets, Events

Before continue in this guide, you must learn, how to implement a database migration, a model, a controller, an event and compiling assets. 
For this study, you can visit these pages:

- Database - [link](modules/database.md)
- Models - [link](modules/models.md)
- Routing - [link](modules/routing.md)
- Controllers - [link](modules/controllers.md)
- Compiling Assets - [link](modules/compiling-assets.md)
- Events - [link](modules/events.md)

It's a good idea to read all pages above before reading the next rows.

## Implementation

