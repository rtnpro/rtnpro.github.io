+++
author = "Ratnadeep Debnath"
categories = ["amd", "fedora 10", "intel", "kvm", "vbox", "virtual box", "virtualization", "rtnpro"]
date = 2009-01-18T22:25:12Z
description = ""
draft = false
slug = "finallyvirtualbox-installed-in-f10"
tags = ["amd", "fedora 10", "intel", "kvm", "vbox", "virtual box", "virtualization", "rtnpro"]
title = "Finally...VirtualBox installed in F10...."

+++


After some googling, I finally got some How-To install VirtualBox in Fedora 10. The problems that I was facing with VirtualBox is that I was unable to compile the kernel module of VirtualBox successfully.  Following are the steps for setting up VirtualBox in a 32-bit Fedora 10 system.

Steps:

**1) Open the terminal and go to the super user mode :**

> su -

**2)Download the latest VirtualBox package for Fedora 9/10 from**

> download.virtualbox.org
> 
> and install it

**3)Get the kernel-devel package :**

> `yum install make automake autoconf gcc kernel-devel dkms`

**4)Run the setup file for VirtualBox :**

> /etc/init.d/vboxdrv setup

**5)Add the desired user-names to the VirtualBox user groups and fix the SE Linux permissions :**

> ```
> usermod -G vboxusers -a username<br></br>
> chcon -t textrel_shlib_t /usr/lib/virtualbox/VirtualBox.so```

**6) Run, and enjoy!**

> `VirtualBox`

**7) To Get USB Support: **

> 1 – create a new group called “usb”;  
>  2 – locate file usbfs: in my case is /sys/bus/usb/drivers (I suggest to find the file with a usb device inserted;  
>  3 – modify file /etc/fstab inserting a line containing the right path and the number corresponding the “usb” group :  
>  none /sys/bus/usb/drivers usbfs devgid=503,devmode=664 0 0  
>  4 – command mount -a;  
>  5 – start VB and try…;

**8 ) Note: VirtualBox is not compatible with kvm, which is an addition in Fedora 10 :**

> Disable the kvm kernel modules before running VirtualBox in the following way
> 
> modprobe -r kvm-intel         (or kvm-amd for amd systems)
> 
> modprobe -r kvm

