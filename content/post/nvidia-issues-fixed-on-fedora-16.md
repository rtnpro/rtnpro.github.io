+++
author = "Ratnadeep Debnath"
categories = ["bug", "bugfix", "f16", "Fedora", "Fedora 16", "glibc", "glibc-2.14.90-4", "nvidia", "NVIDIA-Linux-x86-285.05.09.run", "NVIDIA-Linux-x86-290.06.run", "problem", "ratnadeep debnath", "solved", "rtnpro"]
date = 2011-11-12T13:43:33Z
description = ""
draft = false
slug = "nvidia-issues-fixed-on-fedora-16"
tags = ["bug", "bugfix", "f16", "Fedora", "Fedora 16", "glibc", "glibc-2.14.90-4", "nvidia", "NVIDIA-Linux-x86-285.05.09.run", "NVIDIA-Linux-x86-290.06.run", "problem", "ratnadeep debnath", "solved", "rtnpro"]
title = "NVIDIA issues fixed on Fedora 16"

+++


This week, I upgraded from Fedora 15 to Fedora 16 on my Dell XPS M1530 laptop. This laptop has a 256 MB NVIDIA 8600M GT graphics card. The default driver for NVIDIA cards that came with the installation was nouveau. Nouveau is an open source driver for NVIDIA graphics cards and is under development. Things are becoming better and better with nouveau.

I ran gnome-shell for some time with the nouveau driver. 3d rendering worked nicely and without any latency. I did not find any other issue except for some overheating issues. So, I decided to switch to the NVIDIA’s proprietary driver.

Here is a good tutorial to disable nouveau and install NVIDIA’s proprietary driver in Fedora 16: [http://www.if-not-true-then-false.com/2011/fedora-16-nvidia-drivers-install-guide-disable-nouveau-driver/](http://www.if-not-true-then-false.com/2011/fedora-16-nvidia-drivers-install-guide-disable-nouveau-driver/)

But, this would work perfectly if this was for Fedora 15. Fedora 16 comes with glibc-2.14.90-14 and the NVIDIA proprietary driver (the latest stable driver as of now is NVIDIA-Linux-x86-285.05.09.run). This issue has been reported at [https://bugzilla.redhat.com/show_bug.cgi?id=737223](https://bugzilla.redhat.com/show_bug.cgi?id=737223). The issues I faced after installing the proprietary NVIDIA driver in my Fedora 16 machine were:

- window manager behaving sluggishly
- tab switch in applications like gnome-terminal, nautilus browser taking around 3-4 seconds (no such issues with KDE’s konsole)
- System getting overheated
- Increased latency in gnome-shell effects
- Similar issues with window manager and tab switch on XFCE too

I guess everything depended on glibc were affected.

This issue could be fixed  by downgrading glibc to  glibc-2.14.90-4. I tried to do this to find that there are quite a few applications depended on glibc-2.14.90-14 in F16. So, I gave up the idea. I was looking for nvnews for any news from NVIDIA about fix for the above issue. And I came across this thread [http://www.nvnews.net/vbulletin/showthread.php?t=122606](http://www.nvnews.net/vbulletin/showthread.php?t=122606) where I found about the current releases of NVIDIA graphics driver and the beta driver at [http://www.nvnews.net/vbulletin/showthread.php?p=2498046](http://www.nvnews.net/vbulletin/showthread.php?p=2498046). I was desperate enough to try the beta driver. I did:

- $ wget -ivh [ftp://download.nvidia.com/XFree86/Linux-x86/290.06/NVIDIA-Linux-x86-290.06.run](ftp://download.nvidia.com/XFree86/Linux-x86/290.06/NVIDIA-Linux-x86-290.06.run)
- $ su
- # init 3
- # sh NVIDIA-Linux-x86-290.06.run

and then follow through the on screen instructions.

- # init 5

and logged in. To my surprise, everything was perfect this time. No latency in gnome-shell, no overheating issue. Everything is just fine. Now, I have been running gnome-shell on Fedora 16 in my laptop for over 24 hours. I did not find any issue with the NVIDIA beta graphics driver so far.

