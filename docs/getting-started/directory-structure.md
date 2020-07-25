---
id: directory-structure
title: Directory Structure
sidebar_label: Directory Structure
---

## Introduction

SIMPLO CMS is basing on **[Laravel PHP Framework](https://laravel.com/docs/5.8)**. It means that the default directory structure of **SIMPLO CMS** is the same 
as in Laravel.

> If you want to know more details about **Directory Structure**, you can visit official [Laravel documentation](https://laravel.com/docs/5.8/structure)

## SIMPLO CMS Directory Structure

SIMPLO CMS has custom default directories, which you cannot find in the default directory structure in Laravel. In this section, you can 
find out more about this specific directory structure for SIMPLO CMS.

### The Config Directory

The `config` directory contains default Laravel configuration files, but except of them you can also find some another configuration 
files belongs to SIMPLO CMS. These configuration files can offer you new options, what you can use for your work.

### The Database Directory

The `database` directory contains `factories` directory, where you can see factory classes serving for inserting fake items to 
database tables. It is very useful for testing. In `migrations` directory except of Laravel migration files, there are also some database migrations for SIMPLO CMS. 
The another directory is `seeds`, what is also default directory of Laravel and after when you unpack SIMPLO CMS from the box, 
you will see here already a few seed classes serving for the correct functionality of this system. Everything must be run for the correct install of
SIMPLO CMS.

### The Modules Directory

The `modules` directory contains all available modules, what you can use in your application. A module extends system about new specific 
functionality. If you will need to have something more for your project, you can make your own module in your own template.

### The Public Directory

The `public` directory contains the most important file `index.php`, what is the entry point for application. Except of this file, 
you can see here also compiled asset files such as Javascript, CSS or images mainly for administration panel of SIMPLO CMS. In the future, here will be
also your compiled asset files, what you create for your project in a template.

### The Routes Directory

The `routes` directory contains default files of Laravel, but including them you can find here `admin` directory. This is directory 
with all available routes for administration panel of SIMPLO CMS.

### The Themes Directory

The `themes` directory is one of the most important in SIMPLO CMS, because here you will develop your own project. Everything what you need
for your project, you will do inside this directory.

> **IMPORTANT!** If you do not want to have any problems with future updates of SIMPLO CMS, then you must develop everything strictly inside your
> project's directory located in `themes` directory! If you will miss something important in SIMPLO CMS and it is not possible to extend using modules
> in your project (or any other way here), it will be convenient when you will **let to know us**.
