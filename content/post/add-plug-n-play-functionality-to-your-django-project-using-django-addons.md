+++
author = "Ratnadeep Debnath"
categories = ["diegobz", "django", "django-addons", "glezos", "indifex", "plug-n-play", "python", "ratnadeep debnath", "transifex", "rtnpro"]
date = 2011-10-19T07:17:34Z
description = ""
draft = false
slug = "add-plug-n-play-functionality-to-your-django-project-using-django-addons"
tags = ["diegobz", "django", "django-addons", "glezos", "indifex", "plug-n-play", "python", "ratnadeep debnath", "transifex", "rtnpro"]
title = "Add plug-n-play functionality to your Django project using Django-addons"

+++


> **What is Django-addons?**

A Django app used to add true plug-n-play functionality to your own Django applications and projects. Django-addons is brought to you by [Indifex](http://www.indifex.com), the company behind [Transifex](http://www.transifex.net).

Django-addons is a bunch of code that makes writing addon/plugins for your Django project much easier. Add django-addons to your Django project and you can drop all the addons to ‘/addons’ directory.

> **How to install Django-addons?**

You can install the latest version of django-addons running  
`pip install django-addons`  
 or  
`easy_install django-addons`

You can also install the development version of django-addons with  
`pip install django-addons==dev`  
 or  
`easy_install django-addons==dev`.

> **Source code**

[http://code.indifex.com/django-addons/](http://code.indifex.com/django-addons/)

> **Features**

- Addons overview page
- Automatic signal connecting of addons
- Automatic URL discovery of addons
- Template hooking system (inject code from addons to your main project)
- Django-staticfiles to serve site media from each addon
- Django-notifications support (automatic registration of noticetypes)
- Per addon localization
- Per addon settings
- Disabling addons via ./manage.py addons

