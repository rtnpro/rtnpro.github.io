+++
author = "Ratnadeep Debnath"
categories = ["rtnpro"]
date = 2009-09-05T14:30:00Z
description = ""
draft = false
slug = "finally-completed-writing-fedx-wizard-d"
tags = ["rtnpro"]
title = "Finally, completed writing FedX wizard :D"

+++


<div class="gallery galleryid-116 gallery-columns-3 gallery-size-thumbnail" id="gallery-1"><figure class="gallery-item"><div class="gallery-icon landscape">[![wizard_intro](http://127.0.0.1:8080/wordpress/wp-content/uploads/2009/09/fedx_w1-150x150.png)](http://127.0.0.1:8080/wordpress/2009/09/05/finally-completed-writing-fedx-wizard-d/fedx_w1/)</div><figcaption class="wp-caption-text gallery-caption" id="gallery-1-117"> wizard_intro </figcaption></figure><figure class="gallery-item"><div class="gallery-icon landscape">[![select source & destination](http://127.0.0.1:8080/wordpress/wp-content/uploads/2009/09/fedx_w2-150x150.png)](http://127.0.0.1:8080/wordpress/2009/09/05/finally-completed-writing-fedx-wizard-d/fedx_w2/)</div><figcaption class="wp-caption-text gallery-caption" id="gallery-1-118"> select source & destination </figcaption></figure><figure class="gallery-item"><div class="gallery-icon landscape">[![fedX wizard final page](http://127.0.0.1:8080/wordpress/wp-content/uploads/2009/09/fedx_w3-150x150.png)](http://127.0.0.1:8080/wordpress/2009/09/05/finally-completed-writing-fedx-wizard-d/fedx_w3/)</div><figcaption class="wp-caption-text gallery-caption" id="gallery-1-119"> fedX wizard final page </figcaption></figure></div>Today, on 5th September, 2009, I completed writing the fedX wizard. I didn’t write it from scratch, though. I found the grsync source code, and the GtkAssistant tutorial in [http://www.linuxquestions.org/linux/articles/Technical/New_GTK_Widgets_GtkAssistant](http://www.linuxquestions.org/linux/articles/Technical/New_GTK_Widgets_GtkAssistant) very useful and informative for a newbie like me. I used parts of code from the above mentioned sources. I pushed the wizard.c file today in my gitorious clone of fedX at

> http://gitorious.org/~rtnpro/fedx/rtnpros-clone

The code is not full proof, as is expected, but I will be working on refining the code. If bugs come during its usage, I will try to fix them. The fedX wizard asks the user to enter the source (updated repository in the external storage) and destination (repository in the offline mirror) when fedX is ran for the first time. Then using Glib g_key_file functions, we write this information and other default settings under the session name “fedX” in the grsync.ini file and save the settings. These settings can be updated any time from within grsync or by running the fedX wizard again.

Now, I have started working on the fedX launcher, which will be calling grsync with fedX as the default session. Apart from that, fedX also needs to verify the signatures of the packages in the updated repository to see if they are all from proper sources.

