+++
author = "Ratnadeep Debnath"
categories = ["Dell XPS", "Fedora 12", "M1530", "moblin", "nouveau", "nvidia", "ratnadeep debnath", "rdblacklist", "rtnpro"]
date = 2009-11-24T01:53:52Z
description = ""
draft = false
slug = "installed-fedora-12-on-dell-xps-m1530"
tags = ["Dell XPS", "Fedora 12", "M1530", "moblin", "nouveau", "nvidia", "ratnadeep debnath", "rdblacklist", "rtnpro"]
title = "Installed Fedora 12 on Dell XPS M1530 :)"

+++


Yesterday, I finally completed downloading the Fedora 12 i686 DVD iso. I then tried to install Fedora 12 from iso, as I did when I installed Fedora 11 from its ISO image. But this time, when it came to customizing the packages, it was asking for a network connection to get the softwares from the online repositories ( when no Install Disc was inserted) and when I kept the F11 DVD inserted in my laptop, it took the package list from the F11 DVD. Didn’t expect that.

Then I tried installing F12 from a DVD, and installation was completed successfully. It good to see EXT4 support for GRUB this time. Also, there is the default kms support for NVIDIA cards with the Nouveau driver. But the Nouveau driver is still limited to provide 2D support only ( work is going on for enabling its 3D suport).  Fedora 12 implements improvements in Xorg. The official NVIDIA driver this time comes with customizable PowerMizer and allows to chose among different screen resolutions ( which was missing before till Fedora 11 and the then latest NVIDIA drivers). This not the end of the story for NVIDIA cards. Even after installing Nvidia drivers, nouveau won’t let it start. You have to add the following kernel option for the kernel in which you installed Nvidia :

> rdblacklist=nouveau vga=0x318

The latest official NVIDIA driver is a bit faulty (w.r.t the Xorg implementation in Fedora 12), and does not properly support OpenGL. Compiz does not work, KDE does not behave properly, neither do Moblin Desktop Environment with the current state of NVIDIA drivers.

Fedora 12 implements initramfs (using Dracut) rather than initrd. Boot up has become faster. F12 also allows runtime starting and stopping of bluetooth drivers. NetworkManager has undergone some improvements ( write capabilities to system wide network connections). There has been also a number of improvements in the Virtualization arena (qemu + kvm). Empathy has replaced Pidgin and gnote has replaced Tomboy. The previous plymouth startup screen from F11 has been retained. Fedora has always been developer friendly distro with an array of latest developer tools.

But at the end of the day, even after having an Nvidia graphics card, I can’t turn on the 3d effects. Hope that Nvidia will resolve this problem soon or Nouveau starts supporting 3D. Apart from that everything is fine. I have setup my required devel environment and it’s time to work. I also made a small 300 MB dump of the basic packages I have installed in my system to support multimedia and other things. It has also got a script to autorun the install process. I will soon upload it online.

Have a nice day ![:)](http://127.0.0.1:8080/wordpress/wp-includes/images/smilies/icon_smile.gif)

