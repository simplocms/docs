---
id: directory-structure
title: Directory Structure
sidebar_label: Directory Structure
---

## Introduction

SimploCMS is based on **Laravel PHP Framework** and it means that the default directory structure of **SimploCMS** is the same 
like in Laravel. If you want to know more about the default directory structure, visit official [Laravel documentation](https://laravel.com/docs/5.8/structure).

## SimploCMS Directory Structure

SimploCMS has custom default directories which you cannot find in the default directory structure in Laravel. In this section, you can 
find out more about this specific directory structure for SimploCMS.

### The App Directory

The `app` directory contains default directories for SimploCMS and these are `Components`, `Contracts`, `Helpers`, `Observers`,
`Rules`, `Services`, `Structures` and `Traits`.

### The Config Directory

The `config` directory contains default Laravel configuration files, but except of them you can also find some another configuration 
files belongs to SimploCMS. These configuration files can offer you new options what you can use for your work.

### The Database directory

The `database` directory contains `factories` directory where you can see factory classes serving for inserting fake items to 
database tables. In `migrations` directory except of Laravel migration files, there are also some database migrations for SimploCMS. 
The another directory is `seeds` what is also default directory of Laravel and here you can define new seeds for your application. 
After when you unpack SimploCMS from the box, you will see here already a few seed classes serving for the correct functionality of this 
system.