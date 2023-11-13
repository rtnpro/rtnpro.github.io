+++
author = "Ratnadeep Debnath"
categories = ["fedora 10", "linux", "not working", "nvidia", "plymouth", "xorg.conf", "rtnpro"]
date = 2008-12-19T19:27:36Z
description = ""
draft = false
slug = "fedora-10-issues-with-x-server"
tags = ["fedora 10", "linux", "not working", "nvidia", "plymouth", "xorg.conf", "rtnpro"]
title = "Fedora 10 issues with X Server"

+++


Exams ended, and today in the morning I was heading as fast as I can towards my Mess to get my hands on the Fedora 10 DVD. Also I had to fix some issues regarding another Fedora 10 installation in my friend’s notebook, which is a Compaq Presario with Nvidia graphics card.

The thing that I found there was that his X-server was not running. I tried to peek into the xorg.conf but in vain, it was not there. Then I tried to boot using the kernel option  
 vga=0X318  
 and then Plymouth started working. I was impressed by the booting screen. Alas! It again landed on the CLI, but this time it was much more crisp (might be due to Plymouth). I did google a bit on the issue regarding xorg.conf not present in Fedora 10, I stepped across a method to create the xorg.conf file. I then did the following steps:  
 1)$ su -c “yum install system-config-display”  
 2)$Xorg -configure :1  
 it created a new file xorg.conf.new in the current directory.  
 3)Then I pasted the file as xorg.conf in /etc/X11/ and tried to invoke the X-server, it failed…saying that display not comatible.

Then I downloaded the nvidia drivers from rpmfusion.org…  
 $su -c “yum -y install kmod-nvidia”  
 After installing the nvidia drivers, invoking the X-server worked. But on reboot, it again landed on the CLI. Each time I had to manually invoke the X-server to get to the GUI.  
 I am searching and still couldn’t find a solution to this.

If anybody has any suggestions or ideas on this issue, feel free to post a comment.

