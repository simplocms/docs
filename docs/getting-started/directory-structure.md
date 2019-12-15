---
id: directory-structure
title: Directory Structure
sidebar_label: Directory Structure
---

## Introduction

SIMPLO CMS is based on **[Laravel PHP Framework](https://laravel.com/docs/5.8)** and it means that the default directory structure of **SIMPLO CMS** is the same 
like in Laravel.

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
The another directory is `seeds`, what is also default directory of Laravel and here you can define new seeds for your application. 
After when you unpack SIMPLO CMS from the box, you will see here already a few seed classes serving for the correct functionality of this 
system.

### The Modules Directory

The `modules` directory contains all available modules, what you can use in your application. A module extends system about new specific 
functionality. If you will need to have something more for your project, you can make your own module here. You are completely free.

### The Public Directory

The `public` directory contains the most important file `index.php`, what is the entry point for application. Except of this file, 
you can see here also compiled asset files such as Javascript, CSS or images mainly for administration panel of SIMPLO CMS. In the future, here will be
also your compiled asset files, what you create for your project.

### The Routes Directory

The `routes` directory contains default files of Laravel, but including them you can find here `admin` directory. This is directory 
with all available routes for administration panel of SIMPLO CMS.

### The Themes Directory

The `themes` directory is one of the most important directory in SIMPLO CMS, because here you can create your new theme (project). In the most cases, 
your source code will be inserted in this directory and if you will not need to have more functionality in base SIMPLO CMS, you will not need to change 
source code somewhere outside of this directory or manipulate with something else.
