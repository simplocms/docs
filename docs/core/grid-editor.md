---
id: grid-editor
title: Grid Editor
---

## Introduction

The Grid Editor is a feature, which offers for common users (administrators of course) a good way how they can modify a 
content of some page, for developers as well. It's really simple to use it and we will decribe all options below.

## How To Use Grid Editor

By default, you can find the Grid Editor in administration of pages (under the "Grid" tab). Look in the image below:

![Grid Editor](../assets/images/administration-grid-editor.png "Grid Editor")

First, there is a panel with options for specific devices. It means that you can define the layout separately for more devices according to their width.
The Grid Editor uses Bootstrap layout CSS classes in the resulting HTML for these device options.

On the right side, there is one button for change a current display mode. You can wish to edit a template completely (including rows, cols, etc.) or
just only a content. With this display mode button, you can also see a select list. It's completely history about all changes that have taken place in the past. If you made changes what
you want to get back, there is nothing easier that selecting older content from this list.

### Define A Layout

Under the devices panel, there are main components for making a layout grid. You can add a row with a container, a row without a container or just choose a module.
When you will choose a row with a container, you can see the following grid:

![Grid Editor - a row with a container](../assets/images/administration-grid-editor-row-with-container.png "Grid Editor - a row with a container")

In your new row with a container, you can see three grid components - a container, a row and a column. Each of them offers you a few settings, which we will decribe on the 
following rows.

#### Container 

When you will click on setting icon of the container grid component, there will be possible to set a few options - mainly a class name, element id or
a container element (you can change a default `<div>` element to semantic one - for example `<article>`). Everything you need to change, it's free for you.

When you will necessary need to wrap this container, it's possible to do it using checkbox "Wrap the container" as well. After checked this, it will appear a small box under the checkbox.
Here open for you an option where you can write directly a source code. Be attention that it's not place for insert any special source code. This block serves just only for write a wrapper
where inside this wrapper will be defined the `[container]` wildcard.

![Grid Editor - Container Settings](../assets/images/administration-grid-editor-container-settings.png "Grid Editor - Container Settings")

> **Do not forget** that for correct rendering of a wrapper you must use the `[container]` **wildcard**.

#### Row

After clicking on setting icon of the row grid component, there will appear very similar options like for the container - the main settings are the following few 
options: a class name, element id and a row element as well. Another options are not so interesting but if you need them, then you can use it.

#### Column

When you check the column grid component, here will not be anything else compare with the components above. Just for summary, here are the three main settings: a class name,
element id and a column element as well. Everything you need to change, you are free to do it right here.