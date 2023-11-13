+++
author = "Ratnadeep Debnath"
categories = ["datatables", "django", "html", "indifex", "javascript", "jquery", "pagination", "ratnadeep debnath", "transifex", "rtnpro"]
date = 2011-02-21T10:05:32Z
description = ""
draft = false
slug = "contributing-to-transifex"
tags = ["datatables", "django", "html", "indifex", "javascript", "jquery", "pagination", "ratnadeep debnath", "transifex", "rtnpro"]
title = "Contributing to transifex"

+++


For some time, I have been hanging around with django. I got some cool video tutorials on the web on django. They helped me a lot to start coding in django. At the same time, I was also going through the Transifex source code. After some time, I browsed through the tickets in[ trac.transifex.org](trac.transifex.org) and started with fixing ticket [#679](http://trac.transifex.org/ticket/679).

I had to go through javascript, jquery, html in addition to write the [patch](http://trac.transifex.org/attachment/ticket/679/ticket679.patch) for #679. I also wrote a custom pagination plugin for [dataTables pagination](http://www.datatables.net/plug-ins/pagination) for fixing the issue mentioned in ticket #679. I submitted the patch upstream for review. I hope it will be accepted.

After that Diego directed me to work on ticket [#129](http://trac.transifex.org/ticket/129). Here I had add the provision for a “Remember me” option, which, if selected during login, will set the login duration to 21 days, else, the login session will expire on browser close. I had to create a form class _AuthenticationForm subclassing django’s AuthenticationForm to add a new Boolean form field “Remember me”. Then I had to just pass this _AuthenticationForm as the form argument in simpleauth.views.login and call simpleauth.views.login when the url is of the pattern r’^accounts/login/$’. Yeah … I know this looks odd.

But the best thing about this is that, it works both for simpleauth and for django-profiles. Also, when django-profile is being used, the django-profile templates are used. I have tested this [patch](http://trac.transifex.org/attachment/ticket/129/ticket129.patch) on my system, its working fine. I hope the upstream will accept this.

