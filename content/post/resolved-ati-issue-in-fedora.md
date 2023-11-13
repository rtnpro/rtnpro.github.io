+++
author = "Ratnadeep Debnath"
categories = ["AMD Turion 64 X2", "ati", "ati radeon", "Fedora", "fglrx", "hp business series", "hp compaq 6515b", "kmod", "ratnadeep debnath", "x1250", "akmod", "rtnpro"]
date = 2009-06-24T16:24:02Z
description = ""
draft = false
slug = "resolved-ati-issue-in-fedora"
tags = ["AMD Turion 64 X2", "ati", "ati radeon", "Fedora", "fglrx", "hp business series", "hp compaq 6515b", "kmod", "ratnadeep debnath", "x1250", "akmod", "rtnpro"]
title = "Resolved ATI issue in Fedora"

+++


**System Specifications** :

> hp compaq 6515b,business series
> 
> processor-AMD Turion 64 X2 Dual-Core Mobile Technology  
>  tl-50,clk rate 1600 mhz  
>  hardisk -80gb  
>  ram -1.5gb  
>  Video  
>  Video card features  ATI Radeon X1250  
>  Video Memory Shared video memory (UMA)  
>  Max Allocated RAM Size 512.0 MB

**Issues :**

Problem with the Graphical User Interface. System used to hang a lot. Moving and clicking with the mouse was a pain. Even there was a latency to browse the application menu. Firefox won’t run nicely. System usage was normal. System ran fine in CLI.

Suspected problem : Lack of ATI driver.

**Measures taken :**

*Installed Fedora 10

*Did ssh login to the target system from my laptop to do the post installation setup.

* Installed the basic codecs, kernel packages and other necessary packages to make a kernel module from my local repo.

* Configured rpmfusion on the system.

* Did the following as root :

> * <span>yum –enablerepo=rpmfusion-nonfree-updates-testing install akmod-fglrx xoiorg-x11-drv-fglrx xorg-x11-drv-fglrx-libs.i386 </span>
> 
> <span>* Xorg -configure :1</span>
> 
> <span>*</span><span> aticonfig –initial -f</span>
> 
> <span>* Edit /etc/X11/xorg.conf :</span>
> 
> <span>* Add the following sections or edit accordingly :</span>
> 
> <span>Section “Extensions”  
>  Option “Composite” “Enable”  
>  EndSection</span>
> 
> Section “ServerFlags”  
>  Option “AIGLX” “on”  
>  EndSection
> 
> Section “DRI”  
>  Mode 0666  
>  EndSection
> 
> <span>Add the following lines to the Device Section</span>
> 
> <span>Option        “OpenGLOverlay” “off”  
>  Option        “VideoOverlay” “on”</span>
> 
> <span>*</span><span> mv /boot/initrd-`uname -r`.img /boot/initrd-`uname -r`.img.backup  
>  mkinitrd -v /boot/initrd-`uname -r`.img  `uname -r`</span>
> 
> <span>* Edit the grub.conf file to add “nopat” and “nomodeset” as kernel arguments.</span>
> 
> <span>* Reboot the system. akmod will build the kmod-fglrx for the concerned kernel during this boot. </span>

<span>Results :</span>

<span>Target system was up and running. No more hangs and crashes in the GUI. Installed FEL Packages in the system. Running well.</span>

<span>Reference :</span>

<span>my-guides.net</span>

<span>fedoraguide.info  
</span>

