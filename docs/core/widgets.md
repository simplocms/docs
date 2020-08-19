---
id: widgets
title: Widgets
---

## Introduction

Widget is a component, which offers you to insert an editable often repeated content directly inside a source code. If you want
to make a new widget, you can do it through [Administration Panel](../getting-started/setting.md).

## Implementation

In **Administration panel** you can find **Widgets** page. A content of the widget is created using Grid Editor and including
this content, there are two important indentified fields - identificator and name. Mainly the identificator is very important
and using that you can insert this widget everywhere inside your source code directly.

Widget is working that its name and identificator is common for all available languages. When a widget was created, you can
see it in all language mutators without any limits. The difference between the language mutators is just only a content of the widget. Every time
this content is stored only for an active language.

When you will decide to insert a widget to a source code, you can use the following calling below:

```php
{!! Widget::render('widget_identificator'); !!}
```