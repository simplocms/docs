---
id: setting
title: Setting
sidebar_label: Setting
---

## Introduction

The SIMPLO CMS has many configuration options, which offers you a lot of flexibility and another functionalities. 
As a developer, you can set them using [environment configuration](https://laravel.com/docs/5.8/configuration#environment-configuration) 
with your `.env` file, define new configuration or rewrite default configuration from `config` directory in your theme project. 
Except of these possibilities, you can easily make some changes directly in SIMPLO CMS administration panel, what is very 
comfortable, effective and easy.

> For more information about **Configuration**, you can visit [Configuration](getting-started/configuration.md) page.

## Administration Panel

How it was mensioned in the introduction, you can also set some options using administration panel, which you can visit 
on default page - `your-page.cz/admin`.

> **After first installation of SIMPLO CMS**, the default admin credentials are following: <br><br>
Username: **root** <br>
Password: **RootUser1**

*Administration Panel*

![Administration Panel](../assets/images/administration.png "Administration Panel")

> **TODO** We need to solve localization of administration in SIMPLO CMS !!!

After when you logged in to the Administration Panel, then you can see Dashboard and main menu on the left side. For check actual
settings, you can visit "Settings > General". In the first "General settings" tab, you can set website name, operator, logo, 
icon, background of the icon and also change your active template.

If you will change your actual active template, then in "Template" section there will appear additional settings belonging to 
this active template. For Example template, you can see additional settings in the image below:

![General Settings](../assets/images/administration-general-settings.png "General Settings")

For Example template, it's one additional setting called "Page with articles" serving for setting specific view for rendering 
articles.

The second tab is "SEO" and offers settings for default title and description. In SEO title, you can use variables `%itle%` and
`%site_name%`. When you use them here, on website `%title%` will be replaced with actual title page, what user visits. Variable
`%site_name%` will be replaced in every time with website name, what is the same for the whole web.

The third tab, which you can notice, is "OpenGraph & Twitter". How the name of this tab reveals, here it's possible to set something 
for OpenGraph and Twitter. In the first input, it's place for setting user name to your Twitter account and you have to adduce including
`@`. In the second input, it's convenient place for setting og:title, og:description and og:image. Of course, it's also possible to use
`%title%` and `%site_name%` variables for og:title like in SEO title.

