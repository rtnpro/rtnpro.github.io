+++
author = "Ratnadeep Debnath"
categories = ["rtnpro"]
date = 2009-01-03T10:27:44Z
description = ""
draft = false
slug = "finally-i-installed-fedora-10-in-my-dell-xps-m1530"
tags = ["rtnpro"]
title = "Finally I installed Fedora 10 in my Dell XPS M1530"

+++


Yesterday, on 2nd January, 2009, I finally installed Fedora in my Dell XPS M1530. The installation went fine and it was great to see a new glossy installation screen. After the installation, Plimouth is also working fine, but I had to add a kernel option for that—

> vga=0x318

The graphics was ok , but not perfect without the Nvidia drivers. Then I set rpmfusion.org enabled on my system and downloaded the Nvidia drivers from there.

> su -c ‘yum install kmod-nvidia’

And rebooted my system, and Nvidia is working just fine. Compiz is also working at its best. Plus, the webcam drivers come inbuilt along the new Kernel, GREAT!

But having some problems with the multimedia keys. In Gnome, all the keys except the eject key is working. But its worse in KDE. None of the multimedia keys are working out there. May be its a bug in KDE 4.1. I’m searching for a solution in Google, but couldn’t locate the solution till now. Apart from that, there was another bug in KDE 4.1. The ntfs volumes won’t mount. The solution was jsut to update the ntfs-3g package, and it was solved.

> su -c ‘yum update ntfs-3g’

Apart from the above mentioned issues, its working great. Boot is faster and beautiful, wifi service is nice, webcam is enabled. Great!

