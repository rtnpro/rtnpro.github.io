+++
author = "Ratnadeep Debnath"
categories = ["DownThemAll", "fedora 11", "firefox", "makefile", "ratnadeep debnath", "repository", "rpmfusion", "rsync", "fedx", "addon", "rtnpro"]
date = 2009-10-04T08:56:02Z
description = ""
draft = false
slug = "a-long-night-fedx-get-rpmfusion-repo"
tags = ["DownThemAll", "fedora 11", "firefox", "makefile", "ratnadeep debnath", "repository", "rpmfusion", "rsync", "fedx", "addon", "rtnpro"]
title = "A long night ... fedX + get rpmfusion repo"

+++


I became desperate yesterday night to do some useful things, anything. Had a long offline Puja Holidays. I was struggling to understand “How to write Makefiles”, and yesterday it clicked and I ended up writing a Makefile for the ongoing fedX project. Though the makefile which I wrote is not that great, but I have a start now. Committed the changes to the fedX repository at

[http://gitorious.org/~rtnpro/fedx/rtnpros-fedx-clone/](http://gitorious.org/~rtnpro/fedx/rtnpros-fedx-clone/)

It was around 4:00 AM in the morning already. Made some changes in my gitorious project clones. Apart from that, I was trying to download the rpmfusion repositories for Fedora 11. I was using rsync on multiple screens for the purpose. But again and again, rsync used to stop downloading after working for sometime. Then I tried a Firefox addon “DownThemAll” and was very happy to see it work. All I needed was to browse the repository page listing all the packages, right click and select DownThemAll. And then I just needed to select where I want to save the downloaded files and start it. It downloads multiple files simultaneously and does not halt when one of the files cannot be downloaded, rather it pauses it and moves to others. It also supports resume, and has an option to skip the already downloaded files.

But I could not download the directories with DownThemAll. I guess it works only for files. Need to do some research on it. Any way, you can try to use DownThemAll. I think you will like it.

Finally, I went to sleep at 4:45 AM with my laptop running DownThemAll to download the rpmfusion repository. When I got up today morning, it was running fine. ![:D](http://127.0.0.1:8080/wordpress/wp-includes/images/smilies/icon_biggrin.gif)

